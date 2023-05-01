# mithril-query-jsdom


Query mithril virtual dom for testing purposes

## Installation

    npm install mithril-query-jsdom --save-dev

## Why this fork?

This fork of mihtril-query was created to serve the needs of quering mithril components through another DOM implementation. Since the original mithril query depends on domino for it's DOM (and domino doesn't support for example getComputedStyle) it's very limited in what purpose it can serve you. We found that JSDOM has more options and flexibility in what you can do, so with some simple changes to the code now you can use mihtril query together with JSDOM for your mithril testing purposes.

## Usage

For basic information about how to use this package you're better off reading the offical mithril-query documentation at their [GitHub](https://github.com/MithrilJS/mithril-query)
The only difference in the methods they are showing there is that now you can pass an instance of JSDOM when you're mounting your components.

### A simple example with Jest, JSDOM and mithril-query-dom
Here I'm using Jest, JSDOM and mithril-query-dom to test a simple component. I'm using ESModule with Jest which requires jumping through some hoops:

```js
import * as mq from 'mithril-query-jsdom';
import jsdom from 'jsdom';
import { describe, test, expect, beforeAll, afterAll, jest} from '@jest/globals';

import {SimpleComponent} from '../src/App.js';

let dom;
beforeAll(() => {
    dom = new jsdom.JSDOM("", {
        pretendToBeVisual: true,
    });
    global.window = dom.window;
    global.document = dom.window.document;
    global.requestAnimationFrame = dom.requestAnimationFrame;
});

afterAll(() => {
    dom.window.close();
});

describe('SimpleComponent should', () => {
    test('contain the top bar', () => {
        const component = mq.default(SimpleComponent, {}, dom.window); // Here we pass dom.window as an argument to the function
        component.should.have('.top-bar');
    });
    test('Should have a background color of red, () => {
        const component = mq.default(SimpleComponent, {}, dom.window);
        const queriedElement = dom.document.querySelector('.simple-component');
        const styles = window.getComputedStyle(queriedElement);
        // do your assertion here
    });
});
```

Notice how we comparing to the normal mihtril-query mounting of a component we pass a third argument which is ```dom.window```. After the component is mounted you can do simple queries with mithril-query or query directly the DOM through dom.window / dom.document. Keep in mind you'll need to query using getElementById / querySelector. You have all the window / document methods that JSDOM supports to unleash your testing creativity. This is very useful if you'd want to test for CSS as domino doesn't support getComputedStyle.

If you skip passing the JSDOM created window object, then mithril-query's default way of working will be to create a DOM with domino js.

## Credits and support

The mitrhil-query team created the library so all thanks for the basic package goes to them
Any questions, issues and bugs you find should be forwarded to them if they're not related to the JSDOM functionality.


