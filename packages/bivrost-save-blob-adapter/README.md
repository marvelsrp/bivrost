# Bivrost save blob adapter

Bivrost adapter. Allows to save api resposne as blob.

```
yarn add bivrost-save-blob-adapter
```

## Usage

With [Bivrost](https://github.com/tuchk4/bivrost):

```js
import DataSource from 'bivrost/data/source';
import bivrostApi from 'bivrost/http/api';
import axiosAdapter from 'bivrost-axios-adapter';
import saveBlobAdapter from 'bivrost-save-blob-adapter';

const api = bivrostApi({
  host: 'localhost',
  adapter: saveBlobAdapter(axiosAdapter()),
});

class UsersDataSource extends DataSource {
  steps = ['api'];

  api = {
    loadAll: api('GET /users'),
  };

  saveUsersJSON(filters) {
    return this.invoke('loadAll', filters).then(download => {
      // call download function and generate filename
      download((url, params, response) => `users-${response.length}.json`);
    });
  }
}

const usersDataSource = new UsersDataSource();

// Will save GET /users response as "users.json" file
usersDataSource.saveUsersJSON().then(() => {
  console.log('file saved');
});
```

Direct calls:

```js
import axios from 'axios';
import axiosAdapter from 'bivrost-axios-adapter';
import saveBlobAdapter from 'bivrost-save-blob-adapter';

const axiosAdapter = axiosAdapter(axios);
const saveBlob = saveBlobAdapter(axiosAdapter);

const requestOptions = {
  method: 'GET',
};

saveBlob('/report/exel', requestOptions).then(download => {
  download(() => 'exel.xls');
}); // browser will save and download response as file
```

---

[Bivrost](https://github.com/tuchk4/bivrost) allows to organize a simple
interface to asyncronous APIs.

#### Other adapters

* [Fetch Adapter](https://github.com/tuchk4/bivrost/tree/master/packages/bivrost-fetch-adapter)
* [Axios Adapter](https://github.com/tuchk4/bivrost/tree/master/packages/bivrost-axios-adapter)
* [Delay Adapter](https://github.com/tuchk4/bivrost/tree/master/packages/bivrost-delay-adapter)
* [LocalStorage Adapter](https://github.com/tuchk4/bivrost/tree/master/packages/bivrost-local-storage-adapter)
* [Save Blob Adapter Adapter](https://github.com/tuchk4/bivrost/tree/master/packages/bivrost-save-blob-adapter)
