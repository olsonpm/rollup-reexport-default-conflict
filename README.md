## example of re-export bug

run `npm i && npm run rollup`

to display the following  

*note the erroneous "Conflicting namespaces" message*

```sh
> rollup-reexport-default-conflict@1.0.0 rollup /home/phil/git-repos/personal/rollup-reexport-default-conflict
> ./node_modules/rollup/bin/rollup --format es index.js

⚠️   Conflicting namespaces: dep1.js re-exports 'default' from both dep2.js (will be ignored) and dep3.js

const namedDep2 = 'this is namedDep2';

const namedDep3 = 'this is namedDep3';



var dep1 = Object.freeze({
        namedDep2: namedDep2,
        namedDep3: namedDep3
});

global.keepDep1Alive = dep1;
```

as for the js files

```js
// index.js
import * as dep1 from './dep1';

global.keepDep1Alive = dep1;


// dep1.js
export * from './dep2';
export * from './dep3';


// dep2.js
export default 'this is dep2';

const namedDep2 = 'this is namedDep2';
export { namedDep2 } ;


// dep3.js
export default 'this is dep3';

const namedDep3 = 'this is namedDep3';
export { namedDep3 } ;
```
