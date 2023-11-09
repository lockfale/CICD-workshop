This Walk Thru contains commands and snippets that will be used in the workshop, and should help save time from manually typing complex commands or copying and pasting from slides which may mangle text.

### URL List


Server Name: <input type="text" id="servername" name="servername" placeholder="mylabserver"/>
<script>
document.addEventListener('DOMContentLoaded', function () {
  var inputField = document.getElementById('servername');
  var outputArea = document.getElementById('outputArea');

  inputField.addEventListener('input', function () {
    outputArea.textContent = inputField.value;
  });
});
</script>

- http://<span id="hostname"></span>:8000


```

```