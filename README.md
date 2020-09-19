# React-native setup

## Init

### Run the commands bellow to create your react-native app

```bash
$ npx react-native init {Project}
$ cd Project
```

## Root import

### Install babel-plugin-root-import

```bash
$ yarn add babel-plugin-root-import --dev
```

### Add this config in babel.config.js

```json

"plugins": [
    [
      "babel-plugin-root-import",
      {
        "rootPathSuffix": "src",
        "rootPathPrefix": "~"
      }
    ]
],
```

## Paths imports

### install dependencies

```bash
$ yarn add tsconfig-paths source-map --dev
```

### create a file named as jsconfig.json and put this json on that

```json
{
    "compilerOptions": {
        "target": "ES6",
        "module": "commonjs",
        "allowSyntheticDefaultImports": true,
        "baseUrl": "./",
        "paths": {
            "~/*": [
                "./src/*"
            ]
        }
    },
    "exclude": [
        "node_modules"
    ]
}

```

## React-navigation

### install dependencies

```bash
$ yarn add @react-navigation/native react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

### install dependencies to stack navigation

```bash
$ yarn add @react-navigation/stack
```

### stack navigation example

```jsx
import { createStackNavigator } from '@react-navigation/stack';

const Stack = createStackNavigator();

function MyStack() {
  return (
    <Stack.Navigator>
      <Stack.Screen name="Home" component={Home} />
      <Stack.Screen name="Notifications" component={Notifications} />
      <Stack.Screen name="Profile" component={Profile} />
      <Stack.Screen name="Settings" component={Settings} />
    </Stack.Navigator>
  );
}
```

## React-redux with persist

### install dependencies for redux

```bash
$ yarn add redux react-redux
```

### install dependencies for persist

```bash
$ yarn add redux-persist @react-native-community/async-storage
```

### config example

### src/index.js

```jsx
import React from 'react';
import {Provider} from 'react-redux';
import {PersistGate} from 'redux-persist/integration/react';

import {store, persistor} from './store';
import Routes from '~/routes';

export default function () {
  return (
    <Provider store={store}>
      <PersistGate loading={null} persistor={persistor}>
        <Routes />
      </PersistGate>
    </Provider>
  );
}
```



##### src/store/index.js

```js
import {createStore} from 'redux';
import {persistStore, persistReducer} from 'redux-persist';
import AsyncStorage from '@react-native-community/async-storage';

import rootReducer from './reducers';

const persistConfig = {
  key: 'root',
  storage: AsyncStorage,
  whitelist: ["module_that_will_be_stored"]
};

const persistedReducer = persistReducer(persistConfig, rootReducer);

export const store = createStore(persistedReducer);
export const persistor = persistStore(store);

```

### src/store/reducers/index.js

```js
import {combineReducers} from 'redux';

import module from './module';

const reducers = combineReducers({
  module,
});

export default reducers;

```

### src/store/reducers/module.js

```js
export const Types = {
  ADD_ITEM: 'ADD_ITEM',
  REMOVE_ITEM: 'REMOVE_ITEM',
};

export default function (state = [], {type, payload}) {
  switch (type) {
    case Types.ADD_ITEM:
      return [...state, payload];
    case Types.REMOVE_ITEM:
      return state.filter((item) => item.id !== payload.id);
    default:
      return state;
  }
}
```

