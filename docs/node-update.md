# Node.js Version Update

## Security Issue

Node.js v7.10 has not received security updates since it's last release on [July 11th, 2017](https://github.com/nodejs/node/blob/master/doc/changelogs/CHANGELOG_V7.md#7.10.1). The environments running Node v7 are insecure and are susceptible to any of the published security vulnerabilities since the last update. Node.js grants developers access to system level processes that can be abused if not secured correctly. Even in an "isolated" environment; systems hosting vulnerable Node environments could be taken advantage of.

## Core Node.js Module Support

There have been significant changes made to Node.js since July 11th, 2017. For a complete list visit the following links:

- [2017](https://nodejs.org/en/blog/year-2017/)
- [2018](https://nodejs.org/en/blog/year-2018/)
- [2019](https://nodejs.org/en/blog/year-2019/)
- [2020](https://nodejs.org/en/blog/year-2020/)

Odd-numbered release lines become unstable after six months. Even-numbered releases are maintained for 30-months. For further details read the [Node.js Releases](https://nodejs.org/en/about/releases/) guidelines.

Security updates aside, some notable major API updates include:

- v8
  - npm v5
  - N-API, async_hooks, WHATWG URL, better promise support (`util.promisify`)
- v10
  - N-API graduates from experimental status
  - generators and async functions gain major support
  - fs/promises released
  - async/await support for readable streams
- v14
  - ECMAScript modules graduates from experimental status
  - ArrayBuffer api added

## Node Package Support

Any npm packages developers should be using are no longer supporting Node.js v7. For example, [simple-oauth2](https://www.npmjs.com/package/simple-oauth2), only tests and deploys for Node.js major versions 12 and 14.
