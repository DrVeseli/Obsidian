

GenerateSystemdFile takes a "nameValue" and "port" and generates a .service file for autorunning apps on Linux

```
func GenerateSystemdFile(nameValue string, port int) error {
	serviceContent := fmt.Sprintf(`[Unit]
Description = %s service

[Service]
Type           = simple
User           = root
Group          = root
LimitNOFILE    = 4096
Restart        = always
RestartSec     = 5s
StandardOutput = append:/root/logs/%s.log
StandardError  = append:/root/logs/%s-error.log
ExecStart      = EXEC COMMAND DEPENDS ON THE LANGUAGE"

[Install]
WantedBy = multi-user.target
`, nameValue, nameValue, nameValue, nameValue, port)

	path := fmt.Sprintf("/lib/systemd/system/%s.service", nameValue)
	return os.WriteFile(path, []byte(serviceContent), 0644)
}
```

GenerateNginxConfig creates an nginx config block for the service

```
func GenerateNginxConfig(nameValue string, port int) error {
	configContent := fmt.Sprintf(`
server {
    listen 80;
    server_name %s.yourdomain.com; # customize this if you have a different naming scheme
    location / {
        proxy_pass http://127.0.0.1:%d;
        # ... other proxy settings ...
    }
}
`, nameValue, port)

	path := fmt.Sprintf("/etc/nginx/sites-available/%s", nameValue)
	return os.WriteFile(path, []byte(configContent), 0644)
}
```

{{{this might not be the latest setup, check on macbook}}}