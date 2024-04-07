# The deferred concept in asynchronous programming


⏳ Deferred objects are closely related to promises. They provide a way to control the state of a promise explicitly. While promises are immutable once created, deferred objects allow you to resolve or reject a promise manually.



📑 In this example, a deferred class is created to understand that better.



💅 This approach also helps to have a cleaner code and without struggles with queues or some other methods.


💬 Please note that in real-world scenarios, practical code would include additional features such as try-catch blocks for error handling. However, for this demonstration, I’ve simplified the code to highlight its main functionality.



## Deferred Class:

The Deferred class manages promises with resolve and reject properties. It creates a promise using the Promise constructor, allowing asynchronous resolution or rejection.

```js
class Deferred {
    constructor() {
        this.resolve = null;
        this.reject = null;
        this.promise = new Promise((resolve, reject) => {
            this.resolve = resolve;
            this.reject = reject;
        });
    }
}

```

## Examples of usage of deferred class:



### Custom HTTP Client:

The HttpClient class initializes with empty headers and a deferred object.

After a delay, it sets custom headers and uses them for HTTP requests.

```js
class HttpClient {
    constructor() {
        this.headers = {};
        this.deferred = new Deferred();
        
        setTimeout(() => {
            // create some cryptokeys or processes which needs a time to finish
            this.headers = { ContentType: "application/octet-stream" };
            this.deferred.resolve(); // resolve promise in specific place
        }, 2000);
    }

    async get(url) {
        if (this.deferred.promise) {
            await this.deferred.promise;
        }
        
        return fetch(url, { headers: this.headers });
    }
}

const httpClient = new HttpClient();
const response = await httpClient.get("https://example.com"); 
```

### Image Loader with Deferred: 

The loadImage(url) function asynchronously loads an image. It resolves the deferred promise when the image loads successfully or rejects it in error.

```js
function loadImage(url) {
    const deferred = new Deferred();

    const img = new Image();
    img.onload = () => {
        deferred.resolve(img);
    };
    img.onerror = (error) => {
        deferred.reject(error);
    };
    img.src = url;

    return deferred.promise;
}

const imageUrl = 'https://example.com/image.jpg';
loadImage(imageUrl)
    .then((loadedImage) => { /* add image to the document */ })
    .catch((error) => { /* show some errors */ });
```






