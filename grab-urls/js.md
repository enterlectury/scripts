# About
A javascript book marklet that will extract all endpoints (starting with /) from your current DOM and from all the all the external script sources embedded on the page.
Just copy the code, and save it as a new bookmark. When you visit the page you want to make a wordlist for, click it and done!
<br><br><br>
# For one page js viewing script on browser

1. Go to Bookmarks Manager (Ctrl + Shift + O in Chrome).<br>
2. Click "Add new bookmark". In name field, enter any name. In URL field, paste the below JavaScript code. Click Save. <br>
3. after 3sec extracted endpoints will be displayed in the new page.
<br><br>
```
javascript:(function(){
    var scripts = document.getElementsByTagName("script"),
        regex = /(?<=(\"|\'|\`))\/[a-zA-Z0-9_?&=\/\-\#\.]*(?=(\"|\'|\`))/g;
    const results = new Set();
    
    for (var i = 0; i < scripts.length; i++) {
        var t = scripts[i].src;
        if (t !== "") {
            fetch(t)
            .then(res => res.text())
            .then(data => {
                let matches = data.matchAll(regex);
                for (let match of matches) results.add(match[0]);
            })
            .catch(err => console.log("An error occurred:", err));
        }
    }

    var pageContent = document.documentElement.outerHTML,
        matches = pageContent.matchAll(regex);
    
    for (const match of matches) results.add(match[0]);

    function openResultsInNewTab() {
        let newTab = window.open();
        newTab.document.write("<html><head><title>Extracted Endpoints</title></head><body><pre>");
        results.forEach(endpoint => newTab.document.write(endpoint + "\n"));
        newTab.document.write("</pre></body></html>");
        newTab.document.close();
    }

    setTimeout(openResultsInNewTab, 3000);
})();
```

<br><br>
# Give a Thumpsup!!!🤞
