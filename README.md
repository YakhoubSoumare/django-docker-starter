# Django Docker Starter

A minimal Django project running inside Docker.
Includes an example HTML template to display a simple landing page with a LinkedIn link.

## Features

* Django 5.x running in Python 3.12-slim
* Environment variables for sensitive settings (SECRET\_KEY, DEBUG, ALLOWED\_HOSTS)
* Ready-to-use Dockerfile
* Example frontend template

## Prerequisites

* [Docker](https://docs.docker.com/get-docker/) installed
* [Docker Hub](https://hub.docker.com/) account (for pushing images)

## Getting Started

### 1. Create the project

```bash
mkdir django-docker-starter
cd django-docker-starter

docker run --rm -v $PWD:/app -w /app python:3.12-slim \
  sh -c "pip install django && django-admin startproject mysite django-docker-starter"
```

### 2. Create `.env` file

Example:

```
	SECRET_KEY=replace-with-your-key
	DEBUG=True
	ALLOWED_HOSTS=*
```

Add `.env` to both `.gitignore` and `.dockerignore`.

### 3. Add `requirements.txt`

```bash
	echo "django" > requirements.txt
```

### 4. Create Dockerfile

```dockerfile
	FROM python:3.12-slim
	WORKDIR /app

	COPY requirements.txt .
	RUN pip install --no-cache-dir -r requirements.txt

	COPY . .

	EXPOSE 8000

	ENTRYPOINT ["python"]
	CMD ["manage.py", "runserver", "0.0.0.0:8000"]
```

### 5. Build and run

```bash
	docker build -t your-dockerhub-username/basic-django-app:latest .
	docker run --rm -p 8000:8000 --env-file .env your-dockerhub-username/basic-django-app:latest
```

Visit: [http://localhost:8000](http://localhost:8000)

### 6. Push to Docker Hub

```bash
	docker login
	docker push your-dockerhub-username/basic-django-app:latest
```

## Frontend Example

This project includes a `templates/index.html` file.
The view in `urls.py` renders this template for the root path (`/`).
You can customize it to display any static content you want.

## License

This project is open-source under the MIT License.
