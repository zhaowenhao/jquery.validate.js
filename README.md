# jquery.validate.js
jquery表单验证插件

## 特点

1. 使用灵活，单个表单域可配置，不依赖form标签
1. 每个表单域可以根据验证返回值不同而使用不同的提示语
1. 错误样式可自定义
1. 可失去焦点验证，或关闭该功能
1. 支持后端验证
1. 支持select验证

## 引入

1. 依赖jQuery，请先引入jQuery
1. 直接通过script标签引入

## 使用

1. 引入插件后，会引入validate和validated两个对象级别的方法，使用方法:
    ```javascript
        $('#fieldId').validate(options);
        $('#formId').validated(onSuccess, onFailure);
    ```

2. validate的参数options是一个对象，对象的属性如下：

`options`

| 参数                  | 说明                                                    | 类型                                 | 默认值                       |
| -------------------- | ------------------------------------------------------- | ----------------------------------- | --------------------------- |
| `expression`         | 验证表达式，验证通过返回0，未通过返回大于0的数值                | string &#124; function(options [,callback]){}   |  function() {return 0}      |
| `message1`           | 错误提示语，`message`后面的数字可以是任意值，如果`expression`返回`1`将使用`message1`的提示语，如果返回`2`使用`message2`的提示语，以此类推| string | - |
| `live`               | 是否开启失去焦点验证，select组件则是触发change时验证           | boolean                            | true                      |
| `mode`               | 验证模式，`1`：正常input表单验证，`2`：`expression`函数会有一个回调函数的参数，用于异步校验，`3`：用于select验证 | `1` &#124; `2` &#124; `3` | `1` |
| `error_input_class`  | 验证未通过时，被验证元素添加的样式                            | string                             | 'error'                       |
| `error_message_class`| 验证未通过时，提示语的样式                                  | string                              | 'valid-error'                   |
| `showError`          | 错误提示的显示方式，函数中的this指向当前验证元素               | function(options, fieldResult){}   | 验证未通过时，元素添加`error_input_class`样式，并且在其后面添加一个`error_message_class`样式的div显示提示语 |
| `cleanError`         | 清除错误提示，重写`showError`时，一定要重写该方法             | function(options) {}               | 清除验证元素的`error_input_class`，并清除后面的提示语 |

`expression`当其为函数时会有下列参数

| 参数                  | 说明                                                    | 类型                                 | 默认值                       |
| -------------------- | ------------------------------------------------------- | ----------------------------------- | --------------------------- |
| `options`            | 同`options`                                             | object                              | -                            |
| `callback`           | 回调函数，`mode`为`2`时，必须执行该回调函数，验证通过时调用`callback(0)`，未通过时`callback`的参数是一个大于0的值 | function(number) {} | - |

`showError`默认函数如下

```javascript
    function showError(options, fieldResult) {
        var errorEl = ['<div class="', options.error_message_class, '">', options['message' + fieldResult], '</div>'].join('');
        $(this).addClass(options.error_input_class).after(errorEl);
    }
```

`cleanError`默认函数如下

```javascript
    function cleanError(options) {
        $(this).removeClass(options.error_input_class).next('.' + options.error_message_class).remove();
    }
```

3. validated的两个参数都是函数，整个表单验证通过时回调第一个函数onSuccess，未通过时调用第二个参数onFailure

