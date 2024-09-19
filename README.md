# Javascript--Day-9
## Testing 
16a. In moneyTest.js, add a test case for formatCurrency (2000.4). This test checks if the code rounds down to the nearest cent. After creating the test case, re-run the tests after to see if your code is correct.
```
it('rounds down to the nearest cent', () => {
    expect(formatCurrency(2000.4)).toEqual('20.00');
  });
```
16b. Add another test case for formatCurrency() and test with a negative number (you decide which number to use). Re-run the tests after.
expect() has another method we can use:toHave BeenCalledWith() This checks what values a mocked method received. For example: expect(localStorage.setItem).toHave BeenCalledWith('cart', '[]') Checks if the code called localStorage.setItem('cart', '[]') at some point.
```
it('works with negative numbers', () => {
    expect(formatCurrency(-500)).toEqual('-5.00');
  });
```
16c. In cartTest.js, in the first test, add an expect(localStorage.setItem) and check if setltem received the correct values. The first value should be 'cart' and the second value should be the cart array that was saved (convert the cart array into a string using JSON.stringify since localStorage only supports saving strings).
```
 expect(localStorage.setItem).toHaveBeenCalledWith('cart', JSON.stringify([{
      productId: 'e43638ce-6aa0-4b85-b27f-e1d07eb678c6',
      quantity: 2,
      deliveryOptionId: '1'
    }]));
```
16d. In the second test in cartTest.js, also check if localStorage.setitem() received the correct values.
```
 expect(localStorage.setItem).toHaveBeenCalledWith('cart', JSON.stringify([{
      productId: 'e43638ce-6aa0-4b85-b27f-e1d07eb678c6',
      quantity: 1,
      deliveryOptionId: '1'
    }]));
```
16e. In cartTest.js, create a beforeEach() hook. Put the setup code spyon(localStorage, 'setItem'); in it to share this code between the two tests.
```
beforeEach(() => {
    spyOn(localStorage, 'setItem');
  });

  it('adds an existing product to the cart', () => {
    spyOn(localStorage, 'getItem').and.callFake(() => {
      return JSON.stringify([{
        productId: 'e43638ce-6aa0-4b85-b27f-e1d07eb678c6',
  });
```
16f. In orderSummaryTest.js, create an afterEach() hook. Put the cleanup code document.querySelector('.js-test-container')... in it
```
afterEach(() => {
    document.querySelector('.js-test-container').innerHTML = '';
  });
```
16g. In orderSummaryTest.js, in each test, check if the product name is display correctly on the page. (Hint: in orderSummary.js, you'll need to add a class to the name element in order to get it from the page).
```
<div class="product-name js-product-name-${matchingProduct.id}">
expect(
      document.querySelector(`.js-product-name-${productId1}`).innerText
    ).toEqual('Black and Gray Athletic Cotton Socks - 6 Pairs');
```
16h. In orderSummaryTest.js, check if the product prices are displayed correctly on the page. Each price should have a $ sign in front of it.
```
<div class="product-price js-product-price-${matchingProduct.id}">
 expect(
      document.querySelector(`.js-product-price-${productId1}`).innerText
    ).toEqual('$10.90');
    expect(
      document.querySelector(`.js-product-price-${productId2}`).innerText
    ).toEqual('$20.95');
  });
 expect(
      document.querySelector(`.js-product-price-${productId2}`).innerText
    ).toEqual('$20.95');
```
16i. In cartTest.js, create a test suite for the removeFromCart() function. Mock localStorage.setItem and localStorage.getItem at the start.
Create a test: remove a productid that is in the cart.
Create a test: remove a productid that's not in the cart (does nothing). In each test, check if the cart looks correct. Also, check that localStorage.setItem Was called once and with the correct values. (Note: your code doesn't have to match the solution exactly).
```
import {addToCart, cart, loadFromStorage, removeFromCart} from '../../data/cart.js';

describe('test suite: removeFromCart', () => {
  beforeEach(() => {
    spyOn(localStorage, 'setItem');
  });

  it('removes a product from the cart', () => {
    spyOn(localStorage, 'getItem').and.callFake(() => {
      return JSON.stringify([{
        productId: 'e43638ce-6aa0-4b85-b27f-e1d07eb678c6',
        quantity: 1,
        deliveryOptionId: '1'
      }]);
    });
    loadFromStorage();

    removeFromCart('e43638ce-6aa0-4b85-b27f-e1d07eb678c6');
    expect(cart.length).toEqual(0);
    expect(localStorage.setItem).toHaveBeenCalledTimes(1);
    expect(localStorage.setItem).toHaveBeenCalledWith('cart', JSON.stringify([]));
  });

  it('does nothing if product is not in the cart', () => {
    spyOn(localStorage, 'getItem').and.callFake(() => {
      return JSON.stringify([{
        productId: 'e43638ce-6aa0-4b85-b27f-e1d07eb678c6',
        quantity: 1,
        deliveryOptionId: '1'
      }]);
    });
    loadFromStorage();

    removeFromCart('does-not-exist');
    expect(cart.length).toEqual(1);
    expect(cart[0].productId).toEqual('e43638ce-6aa0-4b85-b27f-e1d07eb678c6');
    expect(cart[0].quantity).toEqual(1);
    expect(localStorage.setItem).toHaveBeenCalledTimes(1);
    expect(localStorage.setItem).toHaveBeenCalledWith('cart', JSON.stringify([{
      productId: 'e43638ce-6aa0-4b85-b27f-e1d07eb678c6',
      quantity: 1,
      deliveryOptionId: '1'
    }]));
  });
```
16j. In orderSummaryTest.js, create a test for updating the delivery option.
In orderSummary.js, add a class to each delivery option element (insert the productid and deliveryOptionld into this class).
Using the DOM, get the 3rd delivery option for the 1st product and click it.
Using the DOM, get the <input> inside this 3rd delivery option (hint: add a class to the <input> with the productid and deliveryOptionld). Test that this <input> is now checked (the.checked property is true).
• Check the cart.length is correct.
Check the first cart item's productid and deliveryOptionld are correct (the deliveryOptionld should be '3' now).
```
<div class="delivery-option js-delivery-option
  js-delivery-option-${matchingProduct.id}-${deliveryOption.id}"
  
  class="delivery-option-input
  js-delivery-option-input-${matchingProduct.id}-${deliveryOption.id}"

<div class="payment-summary-money js-payment-summary-shipping">
  $${formatCurrency(shippingPriceCents)}
</div>

<div class="payment-summary-money js-payment-summary-total">

 it('updates the delivery option', () => {
    document.querySelector(`.js-delivery-option-${productId1}-3`).click();

    expect(
      document.querySelector(`.js-delivery-option-input-${productId1}-3`).checked
    ).toEqual(true);

    expect(cart.length).toEqual(2);
    expect(cart[0].productId).toEqual(productId1);
    expect(cart[0].deliveryOptionId).toEqual('3');

    expect(
      document.querySelector('.js-payment-summary-shipping').innerText
    ).toEqual('$14.98');
    expect(
      document.querySelector('.js-payment-summary-total').innerText
    ).toEqual('$63.50');
  });
```
16k. In cartTest.js, create a test suite for updateDeliveryOption()
Mock localstorage.setItem and localstorage.getItem at the start.
Create a basic test: update the delivery option of a product in the cart.
Check that the cart looks correct. Check localStorage.setItem was called once with the correct values.
```
import {addToCart, cart, loadFromStorage, removeFromCart, updateDeliveryOption} from '../../data/cart.js';

describe('test suite: updateDeliveryOption', () => {
  beforeEach(() => {
    spyOn(localStorage, 'setItem');
  });

  it('updates the delivery option', () => {
    spyOn(localStorage, 'getItem').and.callFake(() => {
      return JSON.stringify([{
        productId: 'e43638ce-6aa0-4b85-b27f-e1d07eb678c6',
        quantity: 1,
        deliveryOptionId: '1'
      }]);
    });
    loadFromStorage();

    updateDeliveryOption('e43638ce-6aa0-4b85-b27f-e1d07eb678c6', '3');
    expect(cart.length).toEqual(1);
    expect(cart[0].productId).toEqual('e43638ce-6aa0-4b85-b27f-e1d07eb678c6');
    expect(cart[0].quantity).toEqual(1);
    expect(cart[0].deliveryOptionId).toEqual('3');

    expect(localStorage.setItem).toHaveBeenCalledTimes(1);
    expect(localStorage.setItem).toHaveBeenCalledWith('cart', JSON.stringify([{
      productId: 'e43638ce-6aa0-4b85-b27f-e1d07eb678c6',
      quantity: 1,
      deliveryOptionId: '3'
    }]));
  });
```
161. In cart.js, modify updateDeliveryOption() so if we give it a productId that is not in the cart, the function will return and do nothing (it does not update the cart and does not save to localStorage).
Create an edge case test: update the delivery option of a product that is not in the cart.
Check the cart looks correct and check that localstorage.setItem was not called.
```
 if (!matchingItem) {
    return;
  }


 it('does nothing if the product is not in the cart', () => {
    spyOn(localStorage, 'getItem').and.callFake(() => {
      return JSON.stringify([{
        productId: 'e43638ce-6aa0-4b85-b27f-e1d07eb678c6',
        quantity: 1,
        deliveryOptionId: '1'
      }]);
    });
    loadFromStorage();

    updateDeliveryOption('does-not-exist', '3');
    expect(cart.length).toEqual(1);
    expect(cart[0].productId).toEqual('e43638ce-6aa0-4b85-b27f-e1d07eb678c6');
    expect(cart[0].quantity).toEqual(1);
    expect(cart[0].deliveryOptionId).toEqual('1');
    expect(localStorage.setItem).toHaveBeenCalledTimes(0);
  });
```
16m. Modify updateDeliveryOption() so if we give it a deliveryOptionld that does not exist, the function will return and do nothing. Hint: to check if a deliveryOptionld exists, use getDeliveryOption() function in deliveryOptions.js and check if the result exists.
Create an edge
case test: use a deliveryOptionld that does not exist. Check the cart looks correct and check that localstorage.setItem was not called.
```
import {validDeliveryOption} from './deliveryOptions.js';

if (!validDeliveryOption(deliveryOptionId)) {
    return;
  }

export function validDeliveryOption(deliveryOptionId) {
  let found = false;

  deliveryOptions.forEach((option) => {
    if (option.id === deliveryOptionId) {
      found = true;
    }
  });

  return found;

it('does nothing if the delivery option does not exist', () => {
    spyOn(localStorage, 'getItem').and.callFake(() => {
      return JSON.stringify([{
        productId: 'e43638ce-6aa0-4b85-b27f-e1d07eb678c6',
        quantity: 1,
        deliveryOptionId: '1'
      }]);
    });
    loadFromStorage();

    updateDeliveryOption('e43638ce-6aa0-4b85-b27f-e1d07eb678c6', 'does-not-exist');
    expect(cart.length).toEqual(1);
    expect(cart[0].productId).toEqual('e43638ce-6aa0-4b85-b27f-e1d07eb678c6');
    expect(cart[0].quantity).toEqual(1);
    expect(cart[0].deliveryOptionId).toEqual('1');
    expect(localStorage.setItem).toHaveBeenCalledTimes(0);
  });
```

