lOnce you've identified a source that lets you add arbitrary properties to the global `Object.prototype`, the next step is to find a suitable gadget that you can use to craft an exploit. In practice, we recommend using DOM Invader to do this, but it's useful to look at the manual process as it may help solidify your understanding of the vulnerability.

1. Look through the source code and identify any properties that are used by the application or any libraries that it imports.
    
2. In Burp, enable response interception (**Proxy > Options > Intercept server responses**) and intercept the response containing the JavaScript that you want to test.
    
3. Add a `debugger` statement at the start of the script, then forward any remaining requests and responses.
    
4. In Burp's browser, go to the page on which the target script is loaded. The `debugger` statement pauses execution of the script.
    
5. While the script is still paused, switch to the console and enter the following command, replacing `YOUR-PROPERTY` with one of the properties that you think is a potential gadget:
    
    `Object.defineProperty(Object.prototype, 'YOUR-PROPERTY', { get() { console.trace(); return 'polluted'; } })`
    
    The property is added to the global `Object.prototype`, and the browser will log a stack trace to the console whenever it is accessed.
    
6. Press the button to continue execution of the script and monitor the console. If a stack trace appears, this confirms that the property was accessed somewhere within the application.
    
7. Expand the stack trace and use the provided link to jump to the line of code where the property is being read.
    
8. Using the browser's debugger controls, step through each phase of execution to see if the property is passed to a sink, such as `innerHTML` or `eval()`.
    
9. Repeat this process for any properties that you think are potential gadgets.