<!-- Сайт розроблено студенткою Ярмолюк Марʼяною, група ФЕМП-5-9з -->
<!doctype html>
<html lang="uk">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Booking — Майстерня / Автомийка</title>
  <meta name="description" content="Сторінка бронювання послуг — приклад студентського проекту.">

  <style>
    :root{--accent:#2b7a78;--muted:#6b6b6b;--bg:#f7f7f9}
    *{box-sizing:border-box}
    body{font-family:Inter, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial; margin:0; background:var(--bg); color:#111}
    header{background:#fff;border-bottom:1px solid #e6e6e6;}
    .container{max-width:1100px;margin:0 auto;padding:20px}
    .nav{display:flex;gap:10px;align-items:center}
    .logo{font-weight:700;color:var(--accent)}
    nav a{padding:10px 12px;border-radius:6px;text-decoration:none;color:#333}
    nav a.active, nav a:hover{background:rgba(43,122,120,0.08)}

    /* layout */
    .hero{display:flex;gap:20px;align-items:center;padding:36px 0}
    .hero .left{flex:1}
    .hero .right{width:320px;background:#fff;border-radius:12px;padding:18px;box-shadow:0 6px 18px rgba(0,0,0,0.04)}
    h1{margin:0 0 8px;font-size:28px}
    p.lead{color:var(--muted);margin:0}

    /* tabs */
    .tabs{display:flex;gap:8px;margin:18px 0}
    .tab-btn{padding:10px 14px;border-radius:8px;background:#fff;border:1px solid #eee;cursor:pointer}
    .tab-btn.active{border-color:var(--accent);color:var(--accent);box-shadow:0 6px 18px rgba(43,122,120,0.06)}

    /* services grid */
    .grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(240px,1fr));gap:16px}
    .card{background:#fff;padding:12px;border-radius:10px;border:1px solid #eee}
    .card img{width:100%;height:140px;object-fit:cover;border-radius:8px}
    .card h3{margin:10px 0 6px;font-size:18px}
    .card p{margin:0 0 10px;color:var(--muted);font-size:14px}
    .card .price{font-weight:700}
    .card .book{display:inline-block;margin-top:6px;padding:8px 12px;border-radius:8px;background:var(--accent);color:#fff;text-decoration:none;cursor:pointer}

    /* booking widget */
    .slot{display:inline-block;padding:6px 8px;border-radius:6px;border:1px solid #eee;margin:6px 6px 0 0;cursor:pointer}
    .slot.selected{background:var(--accent);color:#fff;border-color:var(--accent)}

    footer{padding:18px 0;margin-top:24px;background:#fff;border-top:1px solid #e6e6e6}
    .muted{color:var(--muted);font-size:14px}

    /* modal */
    .modal{position:fixed;inset:0;display:flex;align-items:center;justify-content:center;background:rgba(0,0,0,0.45);visibility:hidden;opacity:0;transition:opacity .18s}
    .modal.show{visibility:visible;opacity:1}
    .modal .panel{background:#fff;padding:20px;border-radius:10px;max-width:520px;width:94%}

    /* responsive */
    @media(max-width:720px){.hero{flex-direction:column}.hero .right{width:100%}}
  </style>
</head>
<body>
  <header>
    <div class="container nav">
      <div class="logo">AutoCare</div>
      <nav style="margin-left:auto" aria-label="Головне меню">
        <a href="#home" class="nav-link" data-link="home">Головна</a>
        <a href="#services" class="nav-link" data-link="services">Послуги</a>
        <a href="#booking" class="nav-link" data-link="booking">Бронювання</a>
        <a href="#about" class="nav-link" data-link="about">Про нас</a>
        <a href="#contact" class="nav-link" data-link="contact">Контакти</a>
      </nav>
    </div>
  </header>

  <main class="container">
    <!-- Sections (реалізовано як вкладки / динамічні блоки) -->

    <section id="home" class="section">
      <div class="hero">
        <div class="left">
          <h1>Швидке та зручне бронювання послуг</h1>
          <p class="lead">Обирайте послугу, вільний часовий слот — отримуйте миттєве підтвердження. Приклад інтерфейсу для студентського проекту.</p>

          <div class="tabs" aria-hidden="true">
            <button class="tab-btn active" data-tab="popular">Популярні</button>
            <button class="tab-btn" data-tab="new">Нові</button>
            <button class="tab-btn" data-tab="discounts">Знижки</button>
          </div>

          <div id="tab-content" style="margin-top:8px"></div>
        </div>

        <aside class="right">
          <strong>Швидке бронювання</strong>
          <p class="muted">Обирайте послугу внизу — потім час та підтверджуйте бронь.</p>

          <div style="margin-top:12px">
            <label class="muted">Вибрана послуга:</label>
            <div id="chosen-service" style="font-weight:700">---</div>
          </div>
        </aside>
      </div>
    </section>

    <section id="services" class="section" style="display:none">
      <h2>Послуги</h2>
      <div class="grid" id="services-grid"></div>
    </section>

    <section id="booking" class="section" style="display:none">
      <h2>Бронювання</h2>
      <div style="display:flex;gap:20px;flex-wrap:wrap">
        <div style="flex:1;min-width:280px">
          <label class="muted">Оберіть послугу</label>
          <select id="service-select" style="width:100%;padding:10px;border-radius:8px;border:1px solid #eee;margin-top:8px"></select>

          <div style="margin-top:12px">
            <label class="muted">Оберіть дату</label>
            <input type="date" id="date-input" style="width:100%;padding:10px;border-radius:8px;border:1px solid #eee;margin-top:8px">
          </div>

          <div style="margin-top:12px">
            <label class="muted">Часові слоти</label>
            <div id="slots" style="margin-top:8px"></div>
          </div>

          <div style="margin-top:14px">
            <button id="confirm-book" style="padding:10px 14px;border-radius:8px;background:var(--accent);color:#fff;border:none">Підтвердити бронювання</button>
          </div>
        </div>

        <div style="flex:1;min-width:240px;background:#fff;padding:12px;border-radius:10px;border:1px solid #eee;align-self:start">
          <h3>Підсумок</h3>
          <p class="muted">Поки що все зберігається тільки в інтерфейсі — без збереження на сервері.</p>
          <div style="margin-top:8px">
            <div><strong>Послуга:</strong> <span id="sum-service">-</span></div>
            <div><strong>Дата:</strong> <span id="sum-date">-</span></div>
            <div><strong>Час:</strong> <span id="sum-time">-</span></div>
            <div style="margin-top:8px"><strong>Ціна:</strong> <span id="sum-price">-</span></div>
          </div>
        </div>
      </div>
    </section>

    <section id="about" class="section" style="display:none">
      <h2>Про нас</h2>
      <p>Автосервіс та автомийка "AutoCare" — демонстраційний проект для навчальної роботи. Інтерфейс моделює процес вибору послуги та бронювання часу. Всі операції виконуються на стороні клієнта.</p>
    </section>

    <section id="contact" class="section" style="display:none">
      <h2>Контакти</h2>
      <p class="muted">Телефон: +380 (00) 000-00-00</p>
      <p class="muted">Адреса: місто, вулиця, 1</p>
    </section>

  </main>

  <footer>
    <div class="container" style="display:flex;justify-content:space-between;align-items:center">
      <div class="muted">Автор: Володимир Токар — Група: ФІТ-2-2</div>
      <div class="muted">© AutoCare</div>
    </div>
  </footer>

  <!-- modal підтвердження -->
  <div id="modal" class="modal" role="dialog" aria-modal="true">
    <div class="panel">
      <h3 id="modal-title">Підтвердження бронювання</h3>
      <p id="modal-body" style="color:var(--muted)"></p>
      <div style="text-align:right;margin-top:12px">
        <button id="modal-close" style="padding:8px 10px;border-radius:8px;margin-right:8px">Закрити</button>
        <button id="modal-ok" style="padding:8px 10px;border-radius:8px;background:var(--accent);color:#fff;border:none">ОК</button>
      </div>
    </div>
  </div>

  <script>
    // --- Дані послуг (можна редагувати)
    const services = [
      {id:1, title:'Повна автомийка', desc:'Мийка кузова, пилосос салону, чистка скла', price:350, tag:'popular', img:'https://picsum.photos/seed/carwash/600/400'},
      {id:2, title:'Швидка мийка', desc:'Експрес-програма для економії часу', price:180, tag:'discounts', img:'https://picsum.photos/seed/express/600/400'},
      {id:3, title:'Полірування кузова', desc:'Глибоке полірування та захист лаку', price:1200, tag:'popular', img:'https://picsum.photos/seed/polish/600/400'},
      {id:4, title:'Ремонт дрібних пошкоджень', desc:'Мелкий ремонт кузова, фарбування', price:900, tag:'new', img:'https://picsum.photos/seed/repair/600/400'},
      {id:5, title:'Детейлінг інтер'єру', desc:'Глибоке очищення салону і відновлення', price:750, tag:'new', img:'https://picsum.photos/seed/detail/600/400'}
    ];

    // --- Структура сторінок (вкладки)
    const sections = document.querySelectorAll('.section');
    const navLinks = document.querySelectorAll('.nav-link');

    function showSection(id){
      sections.forEach(s=>s.style.display = s.id===id ? '' : 'none');
      navLinks.forEach(a=>a.classList.toggle('active', a.dataset.link===id));
      // set hash without reloading
      history.replaceState(null, '', '#'+id);
    }

    // initialize nav from hash or default
    const start = location.hash.replace('#','') || 'home';
    showSection(start);

    navLinks.forEach(a=>a.addEventListener('click', (e)=>{e.preventDefault(); showSection(a.dataset.link);}));

    // --- Render services in grid and tabs
    const grid = document.getElementById('services-grid');
    const tabContent = document.getElementById('tab-content');
    const tabButtons = document.querySelectorAll('.tab-btn');

    function renderGrid(filter){
      grid.innerHTML = '';
      const list = services.filter(s=>!filter || s.tag===filter);
      list.forEach(s=>{
        const card = document.createElement('div'); card.className='card';
        card.innerHTML = `<img src="${s.img}" alt="${s.title}"><h3>${s.title}</h3><p>${s.desc}</p><div><span class="price">${s.price} грн</span> <a class="book" data-id="${s.id}">Забронювати</a></div>`;
        grid.appendChild(card);
      });
    }

    function renderTabContent(tab='popular'){
      const list = services.filter(s=>s.tag===tab).slice(0,3);
      tabContent.innerHTML = '<div class="grid">'+list.map(s=>`<div class="card"><img src="${s.img}" alt="${s.title}"><h3>${s.title}</h3><p>${s.desc}</p><div class="price">${s.price} грн</div></div>`).join('')+'</div>';
    }

    renderGrid(); renderTabContent();

    tabButtons.forEach(b=>b.addEventListener('click', ()=>{tabButtons.forEach(x=>x.classList.remove('active')); b.classList.add('active'); renderTabContent(b.dataset.tab);}));

    // --- Booking interactions
    const serviceSelect = document.getElementById('service-select');
    const slotsWrap = document.getElementById('slots');
    const dateInput = document.getElementById('date-input');
    const chosenService = document.getElementById('chosen-service');

    function populateServiceSelect(){
      serviceSelect.innerHTML = '';
      services.forEach(s=>{
        const opt = document.createElement('option'); opt.value=s.id; opt.textContent = `${s.title} — ${s.price} грн`; serviceSelect.appendChild(opt);
      });
    }

    populateServiceSelect();

    // Prepopulate date with today
    const today = new Date().toISOString().slice(0,10);
    dateInput.value = today;

    // fixed time slots
    const fixedSlots = ['09:00','10:00','11:00','12:00','13:00','14:00','15:00','16:00'];
    let selectedSlot = null;

    function renderSlots(){
      slotsWrap.innerHTML = '';
      fixedSlots.forEach(t=>{
        const el = document.createElement('span'); el.className='slot'; el.textContent=t; el.dataset.time=t;
        el.addEventListener('click', ()=>{
          slotsWrap.querySelectorAll('.slot').forEach(x=>x.classList.remove('selected'));
          el.classList.add('selected'); selectedSlot=t; updateSummary();
        });
        slotsWrap.appendChild(el);
      });
    }

    renderSlots();

    // clicking "Забронювати" на картці відкриває booking секцію та обирає послугу
    document.body.addEventListener('click', (e)=>{
      if(e.target.matches('.book')){
        const id = +e.target.dataset.id; serviceSelect.value = id; chosenService.textContent = services.find(s=>s.id===id).title; showSection('booking'); updateSummary();
      }
    });

    function updateSummary(){
      const sid = +serviceSelect.value; const svc = services.find(s=>s.id===sid);
      document.getElementById('sum-service').textContent = svc ? svc.title : '-';
      document.getElementById('sum-price').textContent = svc ? svc.price + ' грн' : '-';
      document.getElementById('sum-date').textContent = dateInput.value || '-';
      document.getElementById('sum-time').textContent = selectedSlot || '-';
      chosenService.textContent = svc ? svc.title : '---';
    }

    serviceSelect.addEventListener('change', updateSummary);
    dateInput.addEventListener('change', updateSummary);

    // confirm booking -> modal confirmation (no persistence)
    const modal = document.getElementById('modal');
    const modalBody = document.getElementById('modal-body');
    const modalTitle = document.getElementById('modal-title');

    document.getElementById('confirm-book').addEventListener('click', ()=>{
      const sid = +serviceSelect.value; const svc = services.find(s=>s.id===sid);
      if(!svc){ alert('Оберіть послугу'); return; }
      if(!selectedSlot){ alert('Оберіть часовий слот'); return; }
      modalTitle.textContent = 'Бронювання підтверджено';
      modalBody.innerHTML = `Ви забронювали <strong>${svc.title}</strong> на дату <strong>${dateInput.value}</strong> о <strong>${selectedSlot}</strong>.<br>Ціна: <strong>${svc.price} грн</strong>.<br><br>Це підтвердження відображається тільки в інтерфейсі — запис не зберігається.`;
      modal.classList.add('show');
    });

    document.getElementById('modal-close').addEventListener('click', ()=>{ modal.classList.remove('show'); });
    document.getElementById('modal-ok').addEventListener('click', ()=>{ modal.classList.remove('show'); });

    // sync initial summary
    updateSummary();

    // render services grid to its section
    renderGrid();

    // enable clicking a service to auto-scroll to services section
    document.getElementById('services-grid').addEventListener('click', (e)=>{ if(e.target.closest('.book')){ /* handled above */ } });

    // accessibility: close modal on Escape
    window.addEventListener('keydown', (e)=>{ if(e.key==='Escape') modal.classList.remove('show'); });

  </script>
</body>
</html>
