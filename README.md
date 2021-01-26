# Strapi plugin vuejs and Quasar Project


<br>

## Install

```sh
npm install @aysnet/qv-strapi
```
'or'

```sh
yarn add @aysnet/qv-strapi
```

## Start now

### New instance
##### Vuejs
```js 
import Vue from 'vue';
import Strapi from 'strapi-sdk-javascript';
const  options = {
    url:'', // by default : 'http://localhost:1337
    storeConfig: {
        cookie: {
        key: 'jwt',
        options: {
          path: '/'
        }
    },
    localStorage:{} // | boolean
    },
    requestConfig: '',
}
const strapi = new Strapi(options)
Vue.use(strapi)
```
---
##### Quasar Project [boot/strapi.js]
```js 
import Vue from 'vue'
import Strapi from '@q-strapi';
import { options } from './../strapi.options'

const strapi = new Strapi(options)
export default ({
  Vue
}) => {
  Vue.prototype.$strapi = strapi
}
 

export {
  strapi
}
```

### usage

## Authentication

To handle authentication in your Vue / quasar app with Strapi, you can:

### Login

```js
await this.$strapi.login({identifier:'email', password:'password'})
```

### Register

```js
await this.$strapi.register({email:'email', username:'username', password:'password'} )
```

### Logout

```js
await this.$strapi.logout()
```

### Forgot password

```js
await this.$strapi.forgotPassword({email:'email'})
```

### Reset password

```js
await this.$strapi.resetPassword({code:this.$route.query.code /*url*/, password:'password', passwordConfirmation:'passwordConfir..'})
```

### User

Once logged in, you can access your `user` everywhere:

```vue
<template>
  <div>
    <p v-if="$strapi.user">
      Logged in
    </p>
  </div>
</template>

<script>
export default {
  computed: {
    user () {
      return this.$strapi.user
    }
  },
  methods: {
    logout () {
      this.$strapi.logout()
      this.$router.push('/')
    }
  }
}
</script>
```




## Entities

You can access the default Strapi CRUD operations by using these methods:

- `find`
- `count`
- `findByid`
- `create`
- `update`
- `delete`
- `searchFiles`
- `findFile`
- `findFiles`
- `upload`
- `fetch`

```js
await this.$strapi.find('products')
```

### Updating current user

<alert type="info">

You often need to update your user, and so on define a custom route in Strapi: `PUT /users/me`.

</alert>

You can use this module to call it this way:

```js
const user = await this.$strapi.$users.update('me', form)
```

And then mutate the `user`:

```js
this.$strapi.user = user
```

### Authentications

#### Local
```js
await strapi.login({identifier:'email', password:'password'});
```

```js
// Redirect your user to the provider's authentication page.
window.location = strapi.getProviderAuthenticationUrl('facebook');
```
Once authorized, the provider will redirects the user to your app with an access token in the URL.
```js
// Complete the authentication: (The SDK will store the access token for you)
await strapi.authenticateProvider('facebook');
```
You can now fetch private APIs
```js
const posts = await strapi.find('posts');
```

### Files management

#### Browser
```js
const form = new FormData();
form.append('files', fileInputElement.files[0], 'file-name.ext');
form.append('files', fileInputElement.files[1], 'file-2-name.ext');
const files = await strapi.upload(form);
```

#### Node.js
```js
const FormData = require('form-data');
const fs = require('fs');
const form = new FormData();
form.append('files', fs.createReadStream('./file-name.ext'), 'file-name.ext');
const files = await strapi.upload(form, {
  headers: form.getHeaders()
});
```


## API

### `Strapi({url, storeConfig, requestConfig})` ..url default: loclahost:1337
### `request(method, url, requestConfig)`
### `register(username:'usernme', email:'email', password:'password' )`
### `login({identifier:'email', password:'password'})`
### `forgotPassword({email:'email'})`
### `resetPassword({code:'code', password:'password', passwordConfirmation:'passwordConfirmation'})`
### `getProviderAuthenticationUrl(provider)`
### `authenticateProvider(provider, params)`
### `setToken(token, comesFromStorage)`
### `clearToken(token)`
### `fechUser()` return data users/me
### `find(entity, params)`
### `findById(entity, id)`
### `count(entity, params)`
### `create(entity, data)`
### `update(entity, id, data)`
### `delete(entity, id)`
### `searchFiles(query)`
### `findFiles(params)`
### `findFile(id)`
### `upload(data)`
##### `graphql(query)` 
###### exemple query qraphql:
```js
 const DataQuery =  await this.$strapi.graphql({
         query: `
           query {
             classes {
               title
                 user{
                   username
                 }
               }
          }
         `
  });
```
## License

MIT
* [Documentation](https://aysnet1.github.io/qv-starpi)

