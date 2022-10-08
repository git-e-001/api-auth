
## Install Process
```bash
git clone https://github.com/git-e-001/api-auth.git
```

```bash
composer install
```
copy `.env.example` to `.env`

set the database name and run the bellow command

```bash
php artisan migrate:refresh --seed 
```
check you database user table

now you can login
http://127.0.0.1:8000/api/auth/login (post method)
`
{
"email": "youremail@gmail.com",
"password": "password"
}
`

http://127.0.0.1:8000/api/auth/me (get method)
http://127.0.0.1:8000/api/auth/photo-upload (post method)


