# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: cartpagewithfixturetest.spec.ts >> test for continue shopping and add new product into cart
- Location: tests\cartpagewithfixturetest.spec.ts:38:5

# Error details

```
Test timeout of 30000ms exceeded.
```

```
Error: locator.click: Test timeout of 30000ms exceeded.
Call log:
  - waiting for getByRole('button', { name: 'Go back Continue Shopping' })

```

# Page snapshot

```yaml
- generic [ref=e3]:
  - generic [ref=e4]:
    - banner [ref=e5]:
      - generic [ref=e6]:
        - generic [ref=e7]:
          - button "Open Menu" [ref=e8] [cursor=pointer]
          - img "Open Menu" [ref=e9]
        - generic [ref=e10]: Swag Labs
        - button "Cart, 1 items" [ref=e13]:
          - generic [ref=e14]: "1"
      - generic [ref=e15]: Your Cart
    - main [ref=e17]:
      - generic [ref=e18]:
        - generic [ref=e19]:
          - generic [ref=e20]: QTY
          - generic [ref=e21]: Description
          - generic [ref=e22]:
            - generic [ref=e23]: "1"
            - generic [ref=e24]:
              - button "View details for Sauce Labs Bolt T-Shirt" [ref=e25] [cursor=pointer]:
                - generic [ref=e26]: Sauce Labs Bolt T-Shirt
              - generic [ref=e27]: Get your testing superhero on with the Sauce Labs bolt T-shirt. From American Apparel, 100% ringspun combed cotton, heather gray with red bolt.
              - generic [ref=e28]:
                - generic [ref=e29]: $15.99
                - button "Remove" [ref=e30] [cursor=pointer]
        - generic [ref=e31]:
          - button "Continue Shopping" [ref=e32] [cursor=pointer]
          - button "Checkout" [ref=e33] [cursor=pointer]
  - contentinfo [ref=e34]:
    - list [ref=e35]:
      - listitem [ref=e36]:
        - link "X" [ref=e37] [cursor=pointer]:
          - /url: https://x.com/saucelabs
      - listitem [ref=e38]:
        - link "Facebook" [ref=e39] [cursor=pointer]:
          - /url: https://www.facebook.com/saucelabs
      - listitem [ref=e40]:
        - link "LinkedIn" [ref=e41] [cursor=pointer]:
          - /url: https://www.linkedin.com/company/sauce-labs/
    - generic [ref=e42]: © 2026 Sauce Labs. All Rights Reserved. Terms of Service | Privacy Policy
```

# Test source

```ts
  1  | import {Page} from "@playwright/test";
  2  | import {BasePage} from "./BasePage";
  3  | //encapsulation= private data + public methods
  4  | 
  5  | export class CartPage extends BasePage
  6  | {
  7  |     //locators
  8  | private readonly productName;
  9  | private readonly removeButton;
  10 | private readonly continueShoppingBtn;
  11 | private readonly checkoutButton;
  12 | private readonly cartoption
  13 | 
  14 |     //constructors
  15 |     constructor(page:Page)
  16 |     {
  17 |         super(page);
  18 |         this.productName=page.locator("div.inventory_item_name");
  19 |         this.removeButton=page.getByRole('button',{name:'Remove'});
  20 |         this.continueShoppingBtn=page.getByRole('button',{name:"Go back Continue Shopping"});
  21 |         this.checkoutButton=page.getByRole('button',{name:"Checkout"});
  22 |         this.cartoption = page.locator("a.shopping_cart_link");
  23 |     }
  24 | 
  25 |     //methods
  26 |     async gotoCartPage(): Promise<void>
  27 |     {
  28 |         await this.cartoption.click();
  29 |     }
  30 | 
  31 |     async getCartProductDetails():Promise<string[]>
  32 |     {
  33 | 
  34 |         return await this.productName.allInnerTexts();
  35 |     }
  36 | 
  37 |     async removeProduct()
  38 |         {
  39 |             await this.removeButton.click();
  40 |         }
  41 |     
  42 |         async doContinueShopping():Promise<void>
  43 |         {
> 44 |             return await this.continueShoppingBtn.click();
     |                                                   ^ Error: locator.click: Test timeout of 30000ms exceeded.
  45 |         }
  46 | 
  47 |         async gotoCheckOutPage(): Promise<void>
  48 |         {
  49 |             return await this.checkoutButton.click();
  50 |         }
  51 | }
```