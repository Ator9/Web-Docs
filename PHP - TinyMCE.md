#### Buffer
```php
ob_start(); // Start output buffering

// INFO !!;

$data = ob_get_contents(); // Store buffer in variable
ob_end_clean(); // End buffering and clean up
echo $data; // will contain the contents
```

#### TinyMCE Autosave
```js
save_onsavecallback: function () {

    var request = $.ajax({
      url: 'tinymce.php?postID='+<?php echo $_GET['postID']; ?>,
      method: 'POST',
      data: { postID: <?php echo $_GET['postID']; ?>, html: $('#tiny_html').val() },
      dataType: 'text'
    });

    request.fail(function( jqXHR, textStatus ) {
        alert('Request failed');
    });
}
```
