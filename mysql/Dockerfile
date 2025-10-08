FROM mysql:8.0
ENV MYSQL_ROOT_PASSWORD=RoboShop@1
COPY db/* /docker-entrypoint-initdb.d/

# 🧠 Why it may fail if you don’t pass -e MYSQL_ROOT_PASSWORD

# If you only put ENV MYSQL_ROOT_PASSWORD=... in your Dockerfile, Docker will bake it into the image. ✅

# Problem: MySQL’s entrypoint script often ignores ENV in the Dockerfile for security reasons, because it expects the password at runtime.

# The entrypoint script requires a root password to be passed when initializing the container.

# If it’s missing, the container will exit immediately with an error like:

# Error: Database is uninitialized and password option is not specified


# That’s why the recommended way is to pass passwords at runtime using -e:

# docker run -d --name mysql \
#   --network JD-network \
#   -e MYSQL_ROOT_PASSWORD=YourPassword \
#   mysql:v1


# ✅ This ensures the entrypoint script has the password it needs to initialize the database properly.

# Docker ENV vs Runtime `-e` for MySQL

## 1️⃣ ENV in Dockerfile vs `-e` at runtime

### a) ENV in Dockerfile

# ```dockerfile
# ENV MYSQL_ROOT_PASSWORD=mysecretpassword

# Sets an environment variable inside the image.

# Every container started from this image will have this variable baked in.

# Problem: Official MySQL Docker entrypoint may ignore ENV from Dockerfile for sensitive info like root password.

# Reason: Security — prevents accidentally baking secrets into images.

# ✅ Result: Sometimes the container starts successfully, sometimes it fails with:

# Error: Database is uninitialized and password option is not specified

# b) -e at runtime

# docker run -d --name mysql \
#   --network JD-network \
#   -e MYSQL_ROOT_PASSWORD=YourPassword \
#   mysql:v1

# Passes the password directly to the container at runtime.

# MySQL entrypoint reads runtime environment variables, initializes the database with the root password, and starts the service.

# Official recommended way.

# ✅ Result: Container will always start successfully with the correct root password.

# 2️⃣ Why “sometimes it works, sometimes it fails”

# Using ENV in Dockerfile may work if MySQL entrypoint reads it in that container version.

# But newer images often ignore Dockerfile ENV for security reasons.

# Hence unreliable — container can fail randomly depending on image version or initialization process.

# | Concept          | Use Case                                                                                 |
# | ---------------- | ---------------------------------------------------------------------------------------- |
# | Dockerfile `ENV` | Sets default variables in the image ✅<br>Good for **non-sensitive config**               |
# | `-e` at runtime  | Pass variables when container starts ✅<br>Required for **sensitive info** like passwords |

# Rule of Thumb: For secrets (passwords, keys), always use -e at runtime.

# | Concept           | Analogy                                                                         |
# | ----------------- | ------------------------------------------------------------------------------- |
# | ENV in Dockerfile | Writing your house key on the front door sign — not secure, may or may not work |
# | `-e` at runtime   | Giving the key to someone when they arrive — secure, guaranteed to work         |
