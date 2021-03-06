# World

*World* is an isolated context for each scenario, exposed to the hooks and steps as `this`.
The default world constructor is:

```javascript
function World({ attach, log, parameters }) {
  this.attach = attach
  this.log = log
  this.parameters = parameters
}
```

* `attach`: function used for adding [attachments](./attachments.md) to hooks/steps
* `log`: function used for [logging](./attachments.md#logging) information from hooks/steps
* `parameters`: object of parameters passed in via the [CLI](../cli.md#world-parameters)

The default can be overridden with `setWorldConstructor`:

```javascript
const { setWorldConstructor } = require('cucumber')
const seleniumWebdriver = require('selenium-webdriver')

function CustomWorld() {
  this.driver = new seleniumWebdriver.Builder()
    .forBrowser('firefox')
    .build()

  // Returns a promise that resolves to the element
  this.waitForElement = function(locator) {
    const condition = seleniumWebdriver.until.elementLocated(locator)
    return this.driver.wait(condition)
  }
}

setWorldConstructor(CustomWorld)
```

**Note:** The World constructor was made strictly synchronous in *[v0.8.0](https://github.com/cucumber/cucumber-js/releases/tag/v0.8.0)*.
