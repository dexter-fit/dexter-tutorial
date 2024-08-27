## Dexter: A Self-Hosted Approach to Automatic CI/CD Using Open-Source Tools on Low-power Devices

**Petr John, Kristýna Zaklová, Juraj Lazúr, Jiří Hynek and Tomáš Hruška**

## Description

The process of software development has widely adopted Continuous Integration and delivery (CI/CD), offering a comprehensive approach to streamlining the development process. Both features are commonly offered by third-party services or cloud-hosted environments, like Amazon AWS or GitHub. While these offerings are widely available and perfect for large projects and companies, they may not be suitable for smaller projects. Factors like the desire to maintain code and deployment locally or cost considerations can drive the search for alternative solutions. This paper presents an infrastructure mostly based on open-source services that address these specific needs. It is suitable for both development and the automatic deployment of small, locally deployed projects. The solution underwent six months of testing in a lab environment on a low-power device, namely the Raspberry Pi 4B, which was used to both build new versions of the software and host it. The solution leverages the GitHub self-hosted runner to build Docker images from the code directly on the target device. The images are then pushed into a local Docker Registry. This ensures that the code can be used on multiple architectures, namely the arm64, and amd64, by simply pulling an appropriate image that is cross-built thanks to Docker buildx. The built images can then be automatically updated to deploy or be deployed on different hardware. The solution is suitable for small projects or teams with constrained budgets thanks to the self-hosting of all the components, the low purchase price, and the power efficiency of low-power devices like the Raspberry Pi.

## Setup Instructions

1. **Install docker based on your platform:** Follow instructions on the [docker documentation](https://docs.docker.com/engine/install/).
2. **Set up base services:** Download [docker-compose.yml](./docker-compose.yml).
3. **Set up nginx proxy:** Download [nginx.conf](./nginx.conf) and [nginx.htpasswd](./nginx.htpasswd). Place both files into the folder `./data/nginx/auth` relative to the directory where you placed the `docker-compose.yml` file.
4. **Replace placeholder:** Replace all the occurrences of `example.com` with your domain name in both `docker-compose.yml` and `nginx.conf`.
5. **Start up base services:** Run `docker compose up -d`.
6. **Generate SSL certificate using Let’s Encrypt:** `docker compose run --rm certbot certonly --webroot --webroot-path /var/www/certbot/ -d example.com`
7. **Check registry UI:** Registry UI Should then be available under `https://example.com/registry`. You should be able to log in using the user `admin` with the password `admin`.
8. **Add self-hosted runners:** Add self-hosted runners using instructions on the [GitHub page](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/adding-self-hosted-runners).
9. **Done:** Everything is set up and you are good to go!

## Additional Details

To expand your service architecture, you can easily add more services by creating additional `docker-compose` files. This approach allows you to manage and deploy multiple services within isolated containers while maintaining efficient resource usage.

Once you have defined your services in their respective `docker-compose.yml` files, you can configure the existing NGINX instance to act as a reverse proxy. NGINX will direct incoming traffic to the appropriate service based on the request's domain name or path, ensuring seamless integration and communication between your services.

For detailed guidance on setting up NGINX as a reverse proxy, refer to [the official NGINX documentation](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/).
