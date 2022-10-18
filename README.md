# build-in-func-for-JavaScript-Devs

## Back to the top
```js
    const bindTop = () => {
       // Method 1: This works, but it doesn't work very well
       window.scrollTo(0, 0)
       document.documentElement.scrollTop = 0;
        
      // Method 2: Scrolling through the timer will be visually smoother, without much lag effect
      const timeTop = setInterval(() => {
        // to control his sliding distance
        document.documentElement.scrollTop = scrollTopH.value -= 50
        // Remember to clear the timer(*) when swiping to the top
        if (scrollTopH.value <= 0) {
          clearInterval(timeTop)
        }
      }, 10)
    }
```

## Copy text
```js
   const copyText = (text) => {
        // clipboardData Copy what you need on the page to the clipboard
        const clipboardData = window.clipboardData
        if (clipboardData) {
          clipboardData.clearData()
          clipboardData.setData('Text', text)
          return true
        } else if (document.execCommand) {  // Note that document.execCommand is deprecated but some browsers still support it. Remember to check the compatibility when using it
          // Get the content to be copied by creating a dom element 
          const el = document.createElement('textarea')
          el.value = text
          el.setAttribute('readonly', '')
          el.style.position = 'absolute'
          el.style.left = '-9999px'
          document.body.appendChild(el)
          el.select()
          // Copy the current content to the clipboard
          document.execCommand('copy')
          // delete el node
          document.body.removeChild(el)
          return true
        }
        return false
      }
      copyText('hello!') // ctrl + v = copyText  | true
```
## Debounce/Throttle

debounce: trigger an event frequently within a specified time, whichever is the last trigger
throttle: an event is triggered frequently within a specified time, but only once
There are many application scenarios such as:

debounce: input search, when users continue to input content, use anti-shake to reduce the number of requests and save request resources

throttle: The scene is generally a button click. 10 clicks in one second will initiate 10 requests. After throttling, if you click multiple times in 1 second, it will only be triggered once.
```js
   Let’s implement

// debounce
    // fn is the function that needs debounce, delay is the timer time
    function debounce(fn,delay){
        let timer =  null  // for saving the timer
        return function () { 
            // If the timer exists, clear the timer and restart the timer
            if(timer){
                clearTimeout(timeout);
            }
            //Set a timer and execute the actual function to be executed after a specified time
            timeout = setTimeout(() => {
               fn.apply(this);
            }, delay);
        }
    }
    
    // throttle
    function throttle(fn) {
      let timer = null; // First set a variable, when the timer is not executed, the default is null
      return function () {
        if (timer) return; // When the timer is not executed, the timer is always false, and there is no need to execute it later
        timer = setTimeout(() => {
          fn.apply(this, arguments);
           // Finally, set the flag to true after setTimeout is executed (key)
           // Indicates that the next cycle can be executed
          timer = null;
        }, 1000);
      };
    }
```

## Filter special characters
```js
    function filterCharacter(str){
        // First set a mode
        let pattern = new RegExp("[`~!@#$^&*()=：”“'。，、？|{}':;'%,\\[\\].<>/?~！@#$……&*（）&;—|{ }【】‘；]")
        let resultStr1 = "";
        for (let j= 0; j< str.length; j++) {
            // Mainly through replace, pattern rules to replace characters with empty and finally spliced in resultStr1
            resultStr1 = resultStr1 + str.substr(j, 1).replace(pattern, '');
        }
        // When the loop ends, return the final result resultStr1
        return resultStr1;
    }
    
    // Example
    filterCharacter('gyaskjdhy12316789#$%^&!@#1=123,./[') // Result: gyaskjdhy123167891123
```



## Commonly used regular judgment
```js
// Check 2-9 characters, false if not matched, true if matched
    const validateName = (name) => {
      const reg = /^[\u4e00-\u9fa5]{2,9}$/;
      return reg.test(name);
    };
// Verify phone number
    const validatePhoneNum = (mobile) => {
      const reg = /^1[3,4,5,6,7,8,9]\d{9}$/;
      return reg.test(mobile);
    };
// Check passwords consisting of 6 to 18 uppercase and lowercase alphanumeric underscores
    const validatePassword = (password) => {
      const reg = /^[a-zA-Z0-9_]{6,18}$/;
      return reg.test(password);
    };
// Initialize array
// The fill() method is a new method in es6 that fills the array with the specified element, which is actually initializing the array with the default content
    const arrList = Array(6).fill()
    console.log(arrList)  // What is printed here is['','','','','','']
```

## Convert RGB to Hex
```js
   function getColorFun(r,g,b) {
       return '#' + ((1 << 24) + (r << 16) + (g << 8) + b).toString(16).slice(1)
    }
    
    getColorFun(178,232,55) // The output here is #b2e837
```
## Check if it is a function
```js
    // Detecting whether it is a function In fact, it is good to write isFunction directly after writing, so as to avoid repeated writing judgments
    const isFunction = (obj) => {
        return typeof obj === "function" && typeof obj.nodeType !== "number" && typeof obj.item !== "function";
    };
```
## Check if it is a safe array
```js
// Check if it is a safe array, if not, return an empty array here with the isArray method
  const safeArray = (array) => {
    return Array.isArray(array) ? array : []
  }
```
## Check if an object is a secure object
```js
//First of all, we need to determine whether the current object is a valid object
    const isVaildObject = (obj) => {
        return typeof obj === 'object' && !Array.isArray(obj) && Object.keys(obj).length
    }
// The above function is used directly here. If it is valid, it will return itself, and if it is invalid, it will return an empty object.
    const safeObject = obj => isVaildObject(obj) ? obj : {}
```
