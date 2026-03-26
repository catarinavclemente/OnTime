# Docker Image Access Error: openeuropa/triple-store-dev Troubleshooting Guide

## Docker Image Access Error: openeuropa/triple-store-dev Troubleshooting Guide

### Problem

When running `docker compose up -d` or `dcup -d`, you encounter:

```
Error response from daemon: pull access denied for openeuropa/triple-store-dev,
repository does not exist or may require 'docker login'
```

### Root Cause

The `openeuropa/triple-store-dev` image **is not publicly available** on:

* **Docker Hub** (returns 404 error)
* **GitHub Container Registry (ghcr.io)** (returns access denied)

This is a **private container image** maintained by OpenEuropa that requires special authentication or is deprecated. Most developers do not have access to this image.

### Solution

Replace the private image with **`openlink/virtuoso-opensource-7:latest`**, which is:

* ✅ 100% publicly available on Docker Hub
* ✅ Fully compatible with the Drupal SPARQL module
* ✅ Same port (8890) and environment variable support
* ✅ Actively maintained and supported

### Step-by-Step Fix

#### 1. Locate Your `docker-compose.yml` File

Find the file in your project root:

```bash
ls -la docker-compose.yml
```

#### 2. Edit the SPARQL Service Configuration

**Find this section:**

```yaml
sparql:
  image: openeuropa/triple-store-dev
  environment:
    - SPARQL_UPDATE=true
    - DBA_PASSWORD=dba
  ports:
    - "8890:8890"
```

**Replace with:**

```yaml
sparql:
  image: openlink/virtuoso-opensource-7:latest
  environment:
    - SPARQL_UPDATE=true
    - DBA_PASSWORD=dba
  ports:
    - "8890:8890"
```

#### 3. Pull the New Image

```bash
docker pull openlink/virtuoso-opensource-7:latest
```

#### 4. Restart Docker Compose

```bash
docker compose down  # Stop old containers
docker compose up -d # Start new containers with the new image
```

#### 5. Verify It's Working

Check that the SPARQL service is running:

```bash
docker compose ps
```

You should see:

```
NAME              IMAGE                                STATUS
sparql-1          openlink/virtuoso-opensource-7:latest  Up X seconds
```

Test SPARQL connectivity:

```bash
curl -X POST http://localhost:8890/sparql \
  -d "query=SELECT * WHERE {?s ?p ?o} LIMIT 1"
```

### Why This Works

| Aspect                    | openeuropa/triple-store-dev   | openlink/virtuoso-opensource-7   |
| ------------------------- | ----------------------------- | -------------------------------- |
| **Availability**          | Private, access denied        | ✅ Public                         |
| **SPARQL Support**        | SPARQL 1.1 Protocol           | ✅ SPARQL 1.1 Protocol            |
| **Port**                  | 8890                          | ✅ 8890                           |
| **Environment Variables** | SPARQL\_UPDATE, DBA\_PASSWORD | ✅ Supported                      |
| **Drupal Module Support** | sparql\_entity\_storage       | ✅ Full support                   |
| **Virtuoso Version**      | OpenEuropa-specific           | ✅ Virtuoso Open Source Edition 7 |

### Troubleshooting

#### Issue: Docker daemon not running

**Error:** `Cannot connect to the Docker daemon`

**Solution:**

* macOS/Windows: Start Docker Desktop
* Linux: `sudo systemctl start docker`

#### Issue: Image still fails to pull

**Steps:**

1. Verify internet connection: `ping hub.docker.com`
2. Clear Docker cache: `docker system prune`
3. Force re-pull: `docker pull openlink/virtuoso-opensource-7:latest --no-cache`
4. Check Docker Hub status: https://www.docker.com/status

#### Issue: SPARQL endpoint not responding

**URL:** `http://localhost:8890/sparql`

**Common fixes:**

* Wait 10-15 seconds for Virtuoso to initialize
* Check container logs: `docker compose logs sparql`
* Verify port is exposed: `docker compose ps` (should show `0.0.0.0:8890->8890`)

### Testing the Fix

Once the container is running, verify SPARQL functionality:

**1. Check Virtuoso Admin Console**

```
http://localhost:8890/conductor
```

Default credentials: `dba` / `dba`

**2. Run a test SPARQL query**

```bash
curl -s "http://localhost:8890/sparql?query=SELECT%20COUNT(**)%20WHERE%20%7B%3Fs%20%3Fp%20%3Fo%7D"
```

**3. Test from PHP/Drupal**

```php
$sparql_uri = 'http://sparql:8890/sparql';
// Your SPARQL queries will now work
```

### For Teams Using This Fix

If you've applied this fix to your project, document it:

1.  **Add a note to README.md:**

    ```markdown
    ## Note on SPARQL Service
    This project uses `openlink/virtuoso-opensource-7` instead of the unavailable `openeuropa/triple-store-dev` image.
    See [SPARQL Setup Guide](path/to/doc) for details.
    ```
2.  **Commit the docker-compose.yml change:**

    ```bash
    git add docker-compose.yml
    git commit -m "chore: replace unavailable openeuropa/triple-store-dev with openlink/virtuoso-opensource-7"
    ```

### Future Considerations

If OpenEuropa releases an updated `triple-store-dev` image:

1. Check Docker Hub: https://hub.docker.com/r/openeuropa/triple-store-dev
2. Verify it's publicly available
3. Test with your Drupal version
4. Update docker-compose.yml if needed

### References

* **Virtuoso Open Source:** https://virtuoso.openlinksw.com/
* **Docker Hub - Virtuoso:** https://hub.docker.com/r/openlink/virtuoso-opensource-7
* **Drupal SPARQL Module:** https://www.drupal.org/project/sparql\_entity\_storage
* **SPARQL 1.1 Protocol:** https://www.w3.org/TR/sparql11-protocol/

### Need Help?

If you're still experiencing issues:

1. Check `docker compose logs sparql` for container errors
2. Verify Docker version: `docker --version`
3. Ensure sufficient disk space: `docker system df`
4. Review the [Virtuoso troubleshooting guide](https://virtuoso.openlinksw.com/documentation/)
