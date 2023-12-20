# REFACTOR : main.js of [main.js](https://github.com/hei-school/cc-d4-rice-cooker-ci-NyAndoMayah/blob/feat/js/js/main.js) in hei-school/cc-d4-rice-cooker-ci-NyAndoMayah

Hello 👋🏼,
For today we are going to check the complexity of this code [main.js](https://github.com/hei-school/cc-d4-rice-cooker-ci-NyAndoMayah/blob/feat/js/js/main.js) and the refactored code stored in the file called **main.js** of this repository.

## REFACTORING
First let's list the steps of refactoring : 
```javascript
export const riceCooker = {
  ricePresent: false,
  riceCooked: false,
  steamingInProgress: false,
  heatingInProgress: false,

  addRice() {
  /* We'll see the rest later*/
```

> The name of the variable, I think if it's a boolean, it should be isRicePresent and so on.

So the refactored code should be : 
```javascript
export const riceCooker = {
  isRicePresent: false,
  isRiceCooked: false,
  isSteamingInProgress: false,
  isHeatingInProgress: false,

  addRice() {
  /* We'll see the rest later*/
```
Now, let's take a look at this portion of code : 
```javascript
addRice() {
    if (!this.ricePresent) {
      this.ricePresent = true;
      console.log('Rice has been added.');
    } else {
      console.log('There\'s already rice in the rice cooker.');
    }
  },
```
I see here a redundance that we could avoid. First, to facilitate the writing ( to avoid return break or idk every time), we will use ``return``.
It will look like this : 
 ```javascript
addRice() {
    if (!this.ricePresent) {
      this.ricePresent = true;
	     return 'Rice has been added.';
    } else {
      return 'There\'s already rice in the rice cooker.';
    }
  },
```
> Now I'll avoid to write else here because if the condition is fullfilled, the addRice function will end there, else it will immediatly execute the other part of the function ( outside the condition). So it will become like this:
 ```javascript
addRice() {
    if (!this.isRicePresent) {
      this.isRicePresent = true;
	     return 'Rice has been added.';
    } return 'There\'s already rice in the rice cooker.';
  },
```

And that's how we can make the code more better. We'll go from this : 
```javascript
 cookRice() {
    if (this.ricePresent && !this.riceCooked) {
      console.log('Cooking rice...');
      this.delaySync(1500);
      this.riceCooked = true;
      console.log('The rice has been cooked!');
    } else if (!this.ricePresent) {
      console.log('Cannot cook. The rice cooker is empty.');
    } else {
      console.log('The rice is already cooked.');
    }
  },

  steam() {
    if (this.ricePresent && !this.steamingInProgress) {
      console.log('Steaming in progress...');
      this.steamingInProgress = true;
      this.delaySync(1500);
      this.steamingInProgress = false;
      console.log('Steaming completed!');
    } else if (!this.ricePresent) {
      console.log('Cannot steam. The rice cooker is empty.');
    } else {
      console.log('Steaming is already in progress.');
    }
  },

  keepWarm() {
    if (this.ricePresent && this.riceCooked && !this.heatingInProgress) {
      console.log('The rice is now being kept warm.');
      this.heatingInProgress = true;
    } else if (!this.ricePresent) {
      console.log('Cannot keep warm. The rice cooker is empty.');
    } else if (!this.riceCooked) {
      console.log('Cannot keep warm. The rice is not cooked.');
    } else {
      console.log('Keeping warm is already in progress.');
    }
  },

  removeRice() {
    if (this.ricePresent && (this.riceCooked || this.heatingInProgress)) {
      this.ricePresent = false;
      this.riceCooked = false;
      this.steamingInProgress = false;
      this.heatingInProgress = false;
      console.log('The rice has been removed from the rice cooker.');
    } else {
      console.log('There\'s no rice to remove or it is not cooked yet.');
    }
  },
```
to this :
```javascript
cookRice() {  
  if (!this.isRicePresent) {  
    return 'Cannot cook. The rice cooker is empty.';  
  }  
  if(!this.isRiceCooked) {  
    this.delaySync(1500);  
 this.isRiceCooked = true;  
 return 'The rice has been cooked!';  
  }  
  return 'The rice is already cooked.';  
},  
  
steam() {  
  if (!this.isRicePresent) {  
    return 'Cannot steam. The rice cooker is empty.';  
  }  
  if (!this.isSteamingInProgress) {  
    this.isSteamingInProgress = true;  
 this.delaySync(1500);  
 this.isSteamingInProgress = false;  
 return 'Steaming completed!';  
  }  return 'Steaming is already in progress.';  
},  
  
keepWarm() {  
  if (!this.isRicePresent) {  
    return 'Cannot keep warm. The rice cooker is empty.';  
  }  
  if (!this.isRiceCooked) {  
    return 'Cannot keep warm. The rice is not cooked.';  
  }  
  if(!this.isHeatingInProgress) {  
    this.isHeatingInProgress = true;  
 return 'The rice is now being kept warm.';  
  }  
  return 'Keeping warm is already in progress.';  
},  
  
removeRice() {  
  if (this.isRicePresent) {  
    this.isRicePresent = false;  
 this.isRiceCooked = false;  
 this.isSteamingInProgress = false;  
 this.isHeatingInProgress = false;  
 return 'The rice has been removed from the rice cooker.';  
  } return 'There\'s no rice to remove or it is not cooked yet.';  
},
```
Now let's take a look at this ***HORRIBLE CODE***:
```javascript
export function simulateRiceCooker() {
  let input;
  const condition = true;

  while (condition) {
    displayMenu();
    input = prompt('Enter your choice: ');

    if (input) {
      const choice = parseInt(input);

      if (!isNaN(choice)) {
        if (choice === 1) {
          riceCooker.addRice();
        } else if (choice === 2) {
          riceCooker.cookRice();
        } else if (choice === 3) {
          riceCooker.steam();
        } else if (choice === 4) {
          riceCooker.keepWarm();
        } else if (choice === 5) {
          riceCooker.removeRice();
        } else if (choice === 6) {
          console.log('Thank you for using the Rice Cooker Simulator. Goodbye!');
          break;
        } else {
          console.log('Invalid choice. Please select a valid option.');
        }
      } else {
        console.log('Invalid input. Please enter a valid number.');
      }
    } else {
      console.log('No input provided.');
    }
  }
```
The repetitive ``else if`` really sucks. So let's transform this to  a switch case wich is more readable and more performant ( we'll see later).
```javascript
export function simulateRiceCooker() {
  let choice;

  while (true) {
    displayMenu();
    choice = +prompt('Enter your choice: ');
    switch (choice) {
      case 1:
        console.log(riceCooker.addRice());
        break;
      case 2:
        console.log(riceCooker.cookRice());
        break;
      case 3:
        console.log(riceCooker.steam());
        break;
      case 4:
        console.log(riceCooker.keepWarm());
        break;
      case 5:
        console.log(riceCooker.removeRice());
        break;
      case 6:
        console.log('Thank you for using the Rice Cooker Simulator. Goodbye!');
        return;
      default:
        console.log('Invalid choice. Please select a valid option.');
        break;
    }
  }
```
Better ! We'll not forget the ``console.log`` because if not there will only be a huge void.

(*I know that the code in main.js of this repository is not exactly like this but at least I've precised it here.* 😀)

## TIME COMPLEXITY AND ALGORITHM COMPLEXITY
Ok, maybe for some people, the way that we write code doesn't really matter. This work, what's the problem. 😃
But there are some few problems that we should know :
- lisibility 
- **complexity and performance** 


Let's look at the time complexity for each part of the code :
### Not refactored
```javascript
import promptSync from 'prompt-sync';
import {displayMenu} from "./menu.js";
const prompt = promptSync();
```
**O(1)** complexity because initialization of *3 variables* and an *assignation*.

```javascript
export const riceCooker = {
  ricePresent: false,
  riceCooked: false,
  steamingInProgress: false,
  heatingInProgress: false,
```
  **O(1)** complexity because of initialization of *1 function* and *4 keys of an object*.
  
  ``` javascript
  addRice() {

    if (!this.ricePresent) {
      this.ricePresent = true;
      console.log('Rice has been added.');
    } else {
      console.log('There\'s already rice in the rice cooker.');
    }
  },
  ```
  **O(1)** complexity because of initalization of *1 key of object*, *two conditions*, *1 variable call*, *1 variable assignment*, *2 functions call*.
  
```javascript
  cookRice() {
    if (this.ricePresent && !this.riceCooked) {
      console.log('Cooking rice...');
      this.delaySync(1500);
      this.riceCooked = true;
      console.log('The rice has been cooked!');
    } else if (!this.ricePresent) {
      console.log('Cannot cook. The rice cooker is empty.');
    } else {
      console.log('The rice is already cooked.');
    }
  },
```
**O(n)** because of :
- 1 object key initiation
- 4 conditions
- 3 variables call
- 4 function call
- 1 loop in the function 
- 1 variable assignation
```javascript
  steam() {
    if (this.ricePresent && !this.steamingInProgress) {
      console.log('Steaming in progress...');
      this.steamingInProgress = true;
      this.delaySync(1500);
      this.steamingInProgress = false;
      console.log('Steaming completed!');
    } else if (!this.ricePresent) {
      console.log('Cannot steam. The rice cooker is empty.');
    } else {
      console.log('Steaming is already in progress.');
    }
  },
```
**O(n)** because of:
- 1 object key initiation
- 4 conditions
- 4 functions call
- 1 loop in delaySync()
- 2 variables assignation
```javascript
  keepWarm() {
    if (this.ricePresent && this.riceCooked && !this.heatingInProgress) {
      console.log('The rice is now being kept warm.');
      this.heatingInProgress = true;
    } else if (!this.ricePresent) {
      console.log('Cannot keep warm. The rice cooker is empty.');
    } else if (!this.riceCooked) {
      console.log('Cannot keep warm. The rice is not cooked.');
    } else {
      console.log('Keeping warm is already in progress.');
    }
  },
  ```
  **O(1)** because of:
  - 1 object key intiation
  - 6 conditions
  - 4 functions call
  - 6 variables call
  - 1 variable assignation
  ```javascript
  removeRice() {
    if (this.ricePresent && (this.riceCooked || this.heatingInProgress)) {
      this.ricePresent = false;
      this.riceCooked = false;
      this.steamingInProgress = false;
      this.heatingInProgress = false;
      console.log('The rice has been removed from the rice cooker.');
    } else {
      console.log('There\'s no rice to remove or it is not cooked yet.');
    }
  },
```
**O(1)** because of :
- 1 object key initiation
- 5 conditions
- 3 variable call
- 4 variable assignement
- 2 function call
```javascript
  delaySync(ms) {
    const start = Date.now();
    while (Date.now() - start < ms) {
    }
  },
};
```
**O(ms)** because of: 
- 1 while loop depending on ms
- 2 variable assignation
- 2 function call
- 2 variable call
```javascript
export function simulateRiceCooker() {
  let input;
  const condition = true;

  while (condition) {
    displayMenu();
    input = prompt('Enter your choice: ');

    if (input) {
      const choice = parseInt(input);

      if (!isNaN(choice)) {
        if (choice === 1) {
          riceCooker.addRice();
        } else if (choice === 2) {
          riceCooker.cookRice();
        } else if (choice === 3) {
          riceCooker.steam();
        } else if (choice === 4) {
          riceCooker.keepWarm();
        } else if (choice === 5) {
          riceCooker.removeRice();
        } else if (choice === 6) {
          console.log('Thank you for using the Rice Cooker Simulator. Goodbye!');
          break;
        } else {
          console.log('Invalid choice. Please select a valid option.');
        }
      } else {
        console.log('Invalid input. Please enter a valid number.');
      }
    } else {
      console.log('No input provided.');
    }
  }
}

simulateRiceCooker();
```
**O(n) + O(ms)**  and we should notice that *switch case is more efficient than if else in many time complexity especially in this case* [explanation here](https://karankeyash.hashnode.dev/time-complexity-in-switchcase-vs-ifelse).

### Refactored 
```javascript
import promptSync from 'prompt-sync';
import {displayMenu} from "./menu.js";
const prompt = promptSync();
```
**O(1)** complexity because initialization of *3 variables* and an *assignation*.
***Same complexity***

```javascript
export const riceCooker = {
  ricePresent: false,
  riceCooked: false,
  steamingInProgress: false,
  heatingInProgress: false,
```
  **O(1)** complexity because of initialization of *1 function* and *4 keys of an object*.
  ***Same complexity***
  ```javascript
  addRice() {
    if (!this.ricePresent) {
      this.ricePresent = true;
      return 'Rice has been added.';
    }
     return 'There\'s already rice in the rice cooker.';
  },
```
**O(1)** because there are only variables and conditions. *But there is only one condition to check so this is better to read*.
***Same complexity***

```javascript
cookRice() {
    if (!this.ricePresent) {
      return 'Cannot cook. The rice cooker is empty.';
    }
    if(!this.riceCooked) {
      this.delaySync(1500);
      this.riceCooked = true;
      return 'The rice has been cooked!';
    }
    return 'The rice is already cooked.';
  },
```
May be **O(1) for the first or the else condition** but can be **O(n) for the second condition**.
***Same complexity***
```javascript
steam() {
    if (!this.ricePresent) {
      return 'Cannot steam. The rice cooker is empty.';
    }
    if (!this.steamingInProgress) {
      this.steamingInProgress = true;
      this.delaySync(1500);
      this.steamingInProgress = false;
      return 'Steaming completed!';
    }  return 'Steaming is already in progress.';
  },
``` 
May be **O(1) for the first or the else condition** but can be **O(n) for the second condition**.
***Same complexity***
```javascript
keepWarm() {
    if (!this.ricePresent) {
      return 'Cannot keep warm. The rice cooker is empty.';
    }
    if (!this.riceCooked) {
      return 'Cannot keep warm. The rice is not cooked.';
    }
    if(!this.heatingInProgress) {
      this.heatingInProgress = true;
      return 'The rice is now being kept warm.';
    }
    return 'Keeping warm is already in progress.';
  },
```
**O(1)** but conditions are few better because the compilator will not check for redundant conditions, and it is more readable.
***Same complexity***
```javascript
removeRice() {
    if (this.ricePresent) {
      this.ricePresent = false;
      this.riceCooked = false;
      this.steamingInProgress = false;
      this.heatingInProgress = false;
      return 'The rice has been removed from the rice cooker.';
    } return 'There\'s no rice to remove or it is not cooked yet.';
  },
```
**O(1)** but conditions are few better because the compilator will not check for redundant conditions, and it is more readable.
***Same complexity***
```javascript
  delaySync(ms) {
    const start = Date.now();
    while (Date.now() - start < ms) {
    }
  },
};
```
**O(ms)** because of: 
- 1 while loop depending on ms
- 2 variable assignation
- 2 function call
- 2 variable call
***Same complexity***
```javascript
export function simulateRiceCooker() {
  let choice;

  while (true) {
    displayMenu();
    choice = +prompt('Enter your choice: ');
      switch (choice) {
        case 1:
          console.log(riceCooker.addRice());
          break;
        case 2:
          console.log(riceCooker.cookRice());
          break;
        case 3:
          console.log(riceCooker.steam());
          break;
        case 4:
          console.log(riceCooker.keepWarm());
          break;
        case 5:
          console.log(riceCooker.removeRice());
          break;
        case 6:
          console.log('Thank you for using the Rice Cooker Simulator. Goodbye!');
          return;
        default:
          console.log('Invalid choice. Please select a valid option.');
          break;
      }
  }
}

simulateRiceCooker();
```
**O(1)+ O(n)** in best case and **O(ms) + O(n)** in the worst case. More readable, less variables initiations. Compilator will not "suffer".
***Same complexity***

---
>***So in conclusion, in a simple code like that, the complexity is not that different, but the refactoring permits the code to be more readable, and more ....linear.***
----
