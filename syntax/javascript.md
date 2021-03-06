## Javascript

### 基本操作

* 使用變數

```
<html>
    <script type="text/javascript">
        var aVariable = 1;
    </script>
<body>
</body>
</html>
```

```
assert js.aVariable == 1
```

* 呼叫方法(method)

```
<html>
    <script type="text/javascript">
        function add(a,b) {
            return a + b;
        }
    </script>
<body>
</body>
</html>
```

```
assert js.add(1, 1) == 2

```

* 使用原生JS

```
assert js."document.title" == "Geb"
```

### 對話視窗
一般常見的對話視窗有alert、confirm、prompt，[AlertAndConfirmSupport](http://www.gebish.org/manual/current/api/geb/js/AlertAndConfirmSupport.html) 這個類別涵蓋了前兩項，由於Geb不鼓勵使用prompt對話視窗，所以不支持這個功能。

#### 常見的方法有
* String withAlert(Closure actions)
* void withNoAlert(Closure actions)
* void withNoConfirm(Closure actions)
* String withConfirm(boolean ok, Closure actions)
    * ok參數預設為true，可省略

#### 範例
* JavascriptSpec

```
package example

import geb.Page
import geb.spock.GebReportingSpec

/**
 * @see geb.js.AlertAndConfirmSupport
 */
class JavascriptSpec extends GebReportingSpec{

    def setup() {
        to JavascriptPage
    }

    def "withAlert"(){
        expect:
            withAlert(wait: true) { showAlert.click() } == "Hello World!"

    }

    def "withNoAlert"(){
        expect:
            withNoAlert { showAlert.click() }
//        java.lang.AssertionError: an unexpected browser alert() was raised (message: Hello World!)
    }

    def "withConfirm"(){
        expect:
            withConfirm(true) { showConfirm.click() } == "Do you like Geb?"
    }
}

class JavascriptPage extends Page{
    static url = "javascript.html"
    static content = {
        showAlert {$("input", name: "showAlert")}
        showConfirm {$("input", name: "showConfirm")}
    }
}

```

* javascript.html

```
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
</head>
<body>
<input type="button" name="showAlert" onclick="alert('Hello World!');" value="showAlert"/>
<input type="button" name="showConfirm" onclick="confirm('Do you like Geb?');" value="showConfirm"/>
</body>
</html>
```

