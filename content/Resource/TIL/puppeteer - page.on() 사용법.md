---
Create at: 2024-01-28
tags:
  - browser-control
  - TIL/Nodejs
---
---

- `EventEmitter.on()` 메서드는 미리 등록 후 사용해야 한다.
  
```js
const puppeteer = require('puppeteer');


async function run() {
    const browser = await puppeteer.launch({ headless: false });
    const page = await browser.newPage();

    // set up a dialog event handler before using !!!
    page.on('dialog', async dialog => {
        console.log(dialog.message());
        if(dialog.message().includes('clear your cart')) {
            console.log(`clicking "Yes" to ${dialog.message()}`);
            await dialog.accept(); // press 'Yes'
        } else {
            await dialog.dismiss(); // press 'No'
        }
    });

    // add something to cart
    await page.goto('https://web-scraping.dev/product/1');
    await page.click('.add-to-cart');

    // try clearing cart which raises a dialog that says "are you sure you want to clear your cart?"
    await page.goto('https://web-scraping.dev/cart');
    await page.waitForSelector('.cart-full .cart-item');
    await page.click('.cart-full .cart-clear');

    // check the cart
    const cartItems = await page.$('.cart-item .cart-title');
    console.log(`items in cart: ${cartItems ? 1 : 0}`); // Should print 0 if no items in cart.

    await browser.close();
}

run();
```

- 관련 링크
	- https://pptr.dev/api/puppeteer.eventemitter