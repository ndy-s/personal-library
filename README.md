# Personal Library
This project is my personal solution to manage digital resources like tutorials, guides, and books that I find interesting. Some resources are free, while others are behind paywalls. Over time, keeping track of everything became chaotic, and my digital storage felt like a tangled web. Finding anything was difficult.

To solve this, I built this personal library system to organize all my resources efficiently. It uses [Laravel](https://laravel.com/) and [Vue.js](https://vuejs.org/) with [Inertia.js](https://inertiajs.com/) for smooth transitions and [Tailwind CSS](https://tailwindcss.com/) for styling. You can create, update, and delete resources, organize them with sub-data, and scroll through your library effortlessly.

## Demo
Here is a glimpse of the website in action:

![personal-library-demo](https://github.com/ndy-s/personal-library/blob/main/assets/demo.gif)

## Installation and Setup
Clone the repository and go into the project directory:
```bash
git clone https://github.com/ndy-s/personal-library.git
cd personal-library
````

Install dependencies:
```bash
composer install
npm install
```

Set up your environment:
```bash
cp .env-example .env
php artisan key:generate
```

Run database migrations:
```bash
php artisan migrate
```

Build the frontend:
```bash
npm run build
```

After building, you can start the Laravel server:
```bash
php artisan serve
```
> [!TIP]
> By running `npm run build` first, the frontend is compiled and ready, so you do not need to run `npm run dev` separately while running the Laravel server.

The library will be empty at first, so you need to add resources manually through the management section. This project is a starting point and can be customized to fit your needs.

## License
MIT
