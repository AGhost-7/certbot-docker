# Certbot Docker
An image based off the official certbot docker image which includes a cron job
to automatically renew certificates. Still requires a bit of manual work to
initially set up. See [official documentation][1] for information on how to set
it up for the first time.

Pulling down the image: `docker pull aghost7/certbot`

## Usage with haproxy
This image is designed so that you run the reverse proxy in a separate
container. To make it work with haproxy, you would include in your
configuration something along the following lines:

```
frontend front-https
	bind *:443 crt /etc/haproxy/certs/example.com.pem

	acl acme_challenge path_beg /.well-known/acme-challenge/
	use backend certbot if acme_challenge

	# You would have more acls, and a default backend in here.

backend certbot
	server certbot-server your_certbot_ip:6000
```

Digitalocean has a [very good tutorial][2] on how to get this working.


[1]: https://github.com/certbot/certbot
[2]: https://www.digitalocean.com/community/tutorials/how-to-secure-haproxy-with-let-s-encrypt-on-ubuntu-14-04
