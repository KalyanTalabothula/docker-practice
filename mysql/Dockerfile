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