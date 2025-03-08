# A step-by-step guide on setting up a private Docker registry using Harbor. This repository provides an end-to-end tutorial covering installation, configuration, security, and best practices for managing container images securely.


# Harbor Installation and Setup (Without TLS)

## Step 1: Download and Extract Harbor Installer
```bash
wget https://github.com/goharbor/harbor/releases/download/v2.9.0/harbor-offline-installer-v2.9.0.tgz
tar -xvzf harbor-offline-installer-v2.9.0.tgz
cd harbor
```

## Step 2: Modify the `harbor.yml` Configuration
```bash
cp harbor.yml.tmpl harbor.yml
nano harbor.yml
```

### Update the following lines in `harbor.yml`:
```yaml
hostname: 192.168.0.21
http:
  port: 8080
```
*(If you want HTTPS, set up a certificate and configure it instead of HTTP.)*

## Step 3: Install Harbor
```bash
sudo ./install.sh
```

## Step 4: Login to Harbor Registry
```bash
docker login 192.168.0.21:8080
```

## Step 5: Tag and Push an Image
### Tag an Image:
```bash
docker tag ubuntu:latest 192.168.0.21:8080/library/ubuntu:latest
```

### Push the Image:
```bash
docker push 192.168.0.21:8080/library/ubuntu:latest
```

## Step 6: Verify on the Web UI
Check the Harbor UI at [http://192.168.0.21:8080](http://192.168.0.21:8080) to see if the image appears.

---

## Extra: Allow Insecure Registry (if required)
If pushing fails due to HTTPS issues, modify the Docker daemon config:

```bash
sudo nano /etc/docker/daemon.json
```

### Add the following:
```json
{
  "insecure-registries": ["192.168.0.21:8080"]
}
```

### Restart Docker:
```bash
sudo systemctl restart docker
```

---

## Step 7: Login to Harbor Web UI
1. Open your browser and go to [http://192.168.0.21:8080](http://192.168.0.21:8080).
2. Login with your admin credentials (`admin / Harbor12345` by default).

## Step 8: Create a Project
1. Click **"Projects"** in the left menu.
2. Click **"New Project"** (top-right corner).
3. Enter the project name: `myproject`.
4. Choose **"Public"** (or **"Private"** if you want authentication).
5. Click **"OK"** to create it.

## Step 9: Retry the Push
Now try pushing your image again:
```bash
docker push 192.168.0.21:8080/library/ubuntu:latest

