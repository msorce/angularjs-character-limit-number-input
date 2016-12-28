# angularjs-character-limit-number-input


```javascript
angular
  .module('app', []);

// // // // // // // // // // // // // // // // // // // //
// //  MAIN CONTROLLER
// // // // // // // // // // // // // // // // // // // //
angular
  .module('app')
  .controller('mainCtrl', mainCtrl);

mainCtrl.$inject = ['$scope'];

function mainCtrl($scope) {
  this.val = 4;
}

// // // // // // // // // // // // // // // // // // // //
// //  NUM-MAX DIRECTIVE
// // // // // // // // // // // // // // // // // // // //

/**
 * @desc number type input directive that limits the character count, and properly handles paste event, it will also focus the submit button when max is reached
 * @example <num-max max="6"></num-max> || <num-max max="ctrl.val-from-controller"></num-max>
 */
angular
  .module('app')
  .directive('numMax', numMax);

mainCtrl.$inject = ['$timeout'];

function numMax($timeout) {
  var directive = {
    scope: {
      limit: '=limit'
    },
    link: link,
    template: '<input type="number" min="0">',
    restrict: 'EA',
    replace: true
  };
  return directive;

  function link(scope, element, attrs) {
    element.on('keydown keyup', handleInput);
    element.on('paste', handlePaste);

    var eLength,
      sub,
      txt;

    function handleInput(e) {
      eLength = element[0].value.length;
      sub = e.target.parentElement.lastElementChild;
      txt = /Backspace|Enter|Tab|Delete|ArrowLeft|ArrowRight|Control|a/;

      if (!element[0].value) element[0].value = "";
      if (e.keycode > 47 && e.keycode < 58) {
        if (eLength >= scope.limit) e.preventDefault();
      } else if (!txt.test(event.key)) {
        if (eLength >= scope.limit) {
          e.preventDefault();
          sub.focus();
        }
      }

    }

    function handlePaste(e) {
      sub = e.target.parentElement.lastElementChild;

      $timeout(function() {
        eLength = element[0].value.length;

        if (!element[0].value) element[0].value = "";
        else if (eLength >= scope.limit) {
          element[0].value = element[0].value.slice(0, scope.limit);
          sub.focus();
        }

      }, 0);
    }

  }

}
```
