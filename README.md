# Subsonic-API <a href="https://www.npmjs.com/package/subsonic-api"><img src="https://img.shields.io/npm/v/subsonic-api?style=flat&colorA=000000&colorB=efefef"/></a> <a href="https://github.com/vMohammad24/subsonic-api/actions/workflows/test.yml"><img src="https://img.shields.io/github/actions/workflow/status/vMohammad24/subsonic-api/test.yml?branch=main&style=flat&colorA=000000"/></a>

A lightweight Subsonic/Opensubsonic Client written in TypeScript. Supports Node, Deno, Bun, and modern Browsers.

## NOTE

This is a fork of [subsonic-api](https://github.com/explodingcamera/subsonic-api), adding some features i need for my project [NaviThingy](https://github.com/vMohammad24/NaviThingy).

## Features

- Supports all Subsonic API methods up to version 1.16.1 / Subsonic 6.1.4.
- Supports most of OpenSubsonic's new API methods.
- Supports both GET and POST requests.

## Installation

```bash
$ npm install @vmohammad/subsonic-api  # npm
$ bun add @vmohammad/subsonic-api      # bun
```

## Example Usage

```ts
import { SubsonicAPI } from "subsonic-api";

const api = new SubsonicAPI({
  url: "https://demo.navidrome.org",
  auth: {
    username: "demo",
    password: "demo",
  },
});

const { randomSongs } = await api.getRandomSongs();
console.log(randomSongs);
```

## API

`subsonic-api` supports all of the Subsonic API methods as documented [here](http://www.subsonic.org/pages/api.jsp), up to API version 1.16.1 / Subsonic 6.1.4. Additionally, most of [OpenSubsonic's new API methods](https://opensubsonic.netlify.app/) are also supported.
All methods return a promise that resolves to the JSON response from the server.

Additionally, the following methods are available:

### `new SubsonicAPI`

```ts
new SubsonicAPI(config: SubsonicConfig)
```

Creates a new SubsonicAPI instance.

```ts
interface SubsonicConfig {
  // The base URL of the Subsonic server, e.g., https://demo.navidrome.org.
  url: string;

  // The authentication details to use when connecting to the server.
  auth:
    | {
        username: string;
        password: string;
      }
    | {
        // See https://opensubsonic.netlify.app/docs/extensions/apikeyauth/
        apiKey: string;
      };

  // A salt to use when hashing the password. Not used if `auth.apiKey` is provided.
  salt?: string;

  // Whether to reuse generated salts. If not provided,
  // a random salt will be generated for each request.
  // Ignored if `salt` is provided or `auth.apiKey` is used.
  reuseSalt?: boolean;

  // Whether to use a POST requests instead of GET requests.
  // Only supported by OpenSubsonic compatible servers with the `formPost` extension.
  post?: boolean;

  // The fetch implementation to use. If not provided, the global fetch will be used.
  fetch?: Fetch;

  // The crypto implementation to use. If not provided, the global WebCrypto object
  // or the Node.js crypto module will be used.
  crypto?: WebCrypto;
}
```

### `navidromeSession`

```ts
subsonicSession(): Promise<SessionInfo>
```

Creates a new Navidrome session

```ts
interface SessionInfo {
  id: string;
  isAdmin: boolean;
  name: string;
  subsonicSalt: string;
  subsonicToken: string;
  token: string;
  username: string;
}
```

### `baseURL`

```ts
baseURL(): string
```

Returns the base URL of the server. Useful for interacting with other APIs like Navidrome's.

### `custom`

```ts
custom(method: string, params: Params): Promise<Response>
```

Allows you to make a custom request to the server.

### `customJSON`

```ts
customJSON<T>(method: string, params: Params): Promise<T>
```

Allows you to make a custom request to the server and parse the response as JSON.
