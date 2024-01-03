

GenerateSystemdFile takes a "nameValue" and "port" and generates a .service file for autorunning apps on Linux
	you need to modify:
	WorkingDirectory = where the app is located 
	StandardOutput = standard logging
	StandardError = error logging
	ExecStart = the command to run the app

```
func GenerateSystemdFile(nameValue string, port int) error {
	serviceContent := fmt.Sprintf(`[Unit]
	Description=%s

	[Service]
	Type=simple
	User=root
	Group=root
	WorkingDirectory=/var/www/casker/caskers/%[1]s
	LimitNOFILE=4096
	Restart=always
	RestartSec=5s
	StandardOutput=append:/var/www/casker/caskers/%[1]s/errors.log
	StandardError=append:/var/www/casker/caskers/%[1]s/errors.log
	ExecStart=/var/www/casker/caskers/%[1]s/myapp serve --http="127.0.0.1:%d"
	
	[Install]
	WantedBy=multi-user.target
	`, nameValue, port)

	// Define the path for the systemd service file
	path := fmt.Sprintf("/lib/systemd/system/%s.service", nameValue)

	// Write the service file content to the file system
	return os.WriteFile(path, []byte(serviceContent), 0644)
}
```

GenerateNginxConfig creates an nginx config block for the service

```
func GenerateNginxConfig(nameValue string, port int) error {
	configContent := fmt.Sprintf(`
		location /%s/ {
			proxy_set_header Connection '';
			proxy_http_version 1.1;
			proxy_read_timeout 360s;
	
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_set_header X-Forwarded-Proto $scheme;
	
			rewrite ^/%[1]s/(.*)$ /$1 break;
			
			proxy_pass http://127.0.0.1:%d/;
		}
	`, nameValue, port)

	path := fmt.Sprintf("/etc/nginx/sites-partials/%s.conf", nameValue)
	return os.WriteFile(path, []byte(configContent), 0644)
}
```

This is used for the sites-partials configuration and requires a name and a port 