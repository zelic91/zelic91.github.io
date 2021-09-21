---
layout: post
title: "Fast track React"
date: 2021-09-21 22:32:21 +0700
categories: rspec ruby
image: /assets/img/fast.jpeg
---

In this post, I will show you the fast way to fire up an React app. Of course, we will use CRA, several awesome libraries such as redux-toolkit, react-redux, axios and so.

<!--more-->

## 1. Start up

Let's find a good directory and fire up the following command:

{% highlight bash %}
# Terminal
npx create-react-app <app-name>
{% endhighlight %}


## 2. Dependencies

You will need checking .gitignore and add in anything that you don't want to include. Run either one of the commands below to install the necessary dependencies:

{% highlight bash %}

npm install react-redux react-router-dom redux-persist @reduxjs/toolkit axios

# OR

yarn add react-redux react-router-dom redux-persist @reduxjs/toolkit axios

{% endhighlight %}


Open `package.json` and verify if following libraries exist under `dependencies` field (versions might not be the same):

{% highlight json %}

...
"@reduxjs/toolkit": "^1.6.1",
"axios": "^0.21.4",
"react-redux": "^7.2.5",
"react-router-dom": "^5.3.0",
"redux-persist": "^6.0.0",
...

{% endhighlight %}


## 3. Project Structure

I'm lazy, so I use the same approach for every app to save time and energy. Within the `src` directory, create following directories:

{% highlight bash %}
cd src && mkdir components store && mkdir store/middleware
{% endhighlight %}

Folder `components` is for React components in various levels: layout, sidebar, main content, ... While `store` is for Redux stuff.

## 4. Redux toolkit

Redux toolkit eases the pain of repetitive coding style required by Redux (in Javascript). You will be surprised that we don't have any reducer/action file/folder, instead, we only have things called `Slice`. It's all you need.

You would need to create a `store.js` file in `store` folder to gather all the slices available in your app.

{% highlight javascript %}

import { combineReducers, configureStore, getDefaultMiddleware } from '@reduxjs/toolkit'
import storage from 'redux-persist/lib/storage';
import { persistStore, persistReducer } from 'redux-persist';
import api from './middleware/api';
import uiSlice from './uiSlice';

const reducers = combineReducers({
    auth: authSlice,
    ui: uiSlice
});

const persistConfig = {
    key: 'root',
    storage
};

const persistedReducer = persistReducer(persistConfig, reducers);

const store = configureStore({
    reducer: persistedReducer,
    middleware: [
        ...getDefaultMiddleware(),
        api
    ]
});

const persistor = persistStore(store);

export { store, persistor };

{% endhighlight %}

## 5. Network middleware

In order to have a solid API call workflow, we have to setup a middleware file which I called `api.js` in folder `store/middleware`.

{% highlight javascript %}

import * as actions from "../api";
import { showMessage } from "../uiSlice";
import axios from "axios";

const defaultHeaders = {
    'Some-Header': process.env.REACT_APP_APP_TOKEN
};

const api = ({ dispatch }) => (next) => async (action) => {
    if (action.type !== actions.apiCallBegan.type) {
        return next(action);
    }

    const { url, method, data, headers, onStart, onSuccess, onError } = action.payload;

    if (onStart) {
        dispatch({ type: onStart });
    }

    next(action);

    try {
        const response = await axios.request({
            baseURL: `${process.env.REACT_APP_API_END_POINT}`,
            url,
            method,
            data,
            headers: { ...defaultHeaders, ...headers }
        });

        dispatch(actions.apiCallSuccess(response.data));

        if (onSuccess) {
            dispatch({ type: onSuccess, payload: response.data });
        }
    } catch (error) {
        dispatch(actions.apiCallFailed({ error: error.message }));
        dispatch(showMessage({ message: error?.response?.data?.error_message }));

        if (onError) {
            dispatch({ type: onError, payload: { error: error.message } });
        }
    }
};

export default api;

{% endhighlight %}

## 6. Environment variables

In the root folder, you can create `.env` and `.env.development` to set environment variables. Remember that every ENV that is used by CRA must be prefixed with `REACT_APP_` and used with `process.env.REACT_APP_`.

