const CACHE_NAME = 'sales-sniper-v11';
const ASSETS_TO_CACHE = [
  './',
  './index.html',
  './manifest.json'
];

// نصب و کش کردن فایل های اصلی
self.addEventListener('install', (e) => {
  e.waitUntil(
    caches.open(CACHE_NAME).then((cache) => {
      return cache.addAll(ASSETS_TO_CACHE);
    })
  );
});

// فعال سازی و پاک کردن کش های قدیمی
self.addEventListener('activate', (e) => {
  e.waitUntil(
    caches.keys().then((keyList) => {
      return Promise.all(keyList.map((key) => {
        if (key !== CACHE_NAME) {
          return caches.delete(key);
        }
      }));
    })
  );
});

// استراتژی: اول کش، اگر نبود شبکه
self.addEventListener('fetch', (e) => {
  e.respondWith(
    caches.match(e.request).then((response) => {
      return response || fetch(e.request);
    })
  );
});
