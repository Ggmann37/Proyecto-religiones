<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Religiones - App</title>
  <style>
    :root{--bg:#f3f4f6;--card:#fff;--muted:#6b7280;--accent:#3b82f6}
    *{box-sizing:border-box;font-family:Inter, ui-sans-serif, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial}
    body{margin:0;background:var(--bg);color:#111}
    .container{max-width:1024px;margin:28px auto;padding:20px}
    .card{background:var(--card);border-radius:14px;box-shadow:0 6px 20px rgba(2,6,23,0.06);padding:20px}
    .grid{display:grid;gap:16px}
    .grid.cols-3{grid-template-columns:repeat(3,1fr)}
    .btn{display:inline-block;padding:10px 14px;border-radius:10px;border:none;background:var(--accent);color:#fff;cursor:pointer}
    .muted{color:var(--muted)}
    .section-btn{padding:18px;border-radius:12px;border:1px solid #e6e7ea;background:#fff;cursor:pointer;text-align:left}
    .section-btn:hover{box-shadow:0 8px 30px rgba(2,6,23,0.06);transform:translateY(-3px)}
    .flex{display:flex;gap:12px;align-items:center}
    .right{margin-left:auto}
    .hidden{display:none}
    .avatar{width:48px;height:48px;border-radius:999px;background:#eee;display:inline-flex;align-items:center;justify-content:center}
    .stars{font-size:20px;cursor:pointer;user-select:none}
    textarea{width:100%;padding:12px;border-radius:10px;border:1px solid #e6e7ea}
    input{padding:10px;border-radius:10px;border:1px solid #e6e7ea;width:100%}
    .list-item{padding:10px;border-radius:10px;border:1px solid #f0f0f0}
    .small{font-size:13px}
    .topbar{display:flex;gap:12px;align-items:center;margin-bottom:18px}
    .center{text-align:center}
    .modal-back{position:fixed;inset:0;background:rgba(0,0,0,0.45);display:flex;align-items:center;justify-content:center}
    .modal{width:90%;max-width:560px;background:#fff;border-radius:12px;padding:18px}
    .close{background:#eee;border-radius:8px;padding:6px;cursor:pointer}
    .file-drop{border:2px dashed #e6e7ea;border-radius:10px;padding:14px;text-align:center;cursor:pointer}
    .small-muted{font-size:13px;color:var(--muted)}
    .admin-list{max-height:300px;overflow:auto}
  </style>
</head>
<body>
  <div class="container">
    <div class="topbar">
      <div id="avatarBtn" class="avatar">👤</div>
      <div>
        <div id="welcomeText" class="small">Bienvenido</div>
        <div id="welcomeId" style="font-weight:700"></div>
      </div>
      <div class="right">
        <button id="logoutBtn" class="btn hidden">Salir</button>
      </div>
    </div>

    <div id="authArea" class="card">
      <div id="loginView">
        <h2>Iniciar sesión</h2>
        <div id="authError" class="small-muted"></div>
        <div style="margin-top:12px">
          <input id="email" placeholder="E-mail" />
        </div>
        <div style="margin-top:8px">
          <input id="userid" placeholder="ID" />
        </div>
        <div style="margin-top:8px">
          <input id="pass" type="password" placeholder="Contraseña" />
        </div>
        <div style="margin-top:12px;display:flex;gap:8px">
          <button id="loginBtn" class="btn">Entrar</button>
          <button id="showRegister" class="btn" style="background:#10b981">Regístrate</button>
          <button id="showAdmin" class="btn" style="background:#f59e0b">Soy Admin</button>
        </div>
      </div>

      <div id="registerView" class="hidden">
        <h2>Crear cuenta</h2>
        <div id="regError" class="small-muted"></div>
        <div style="margin-top:12px"><input id="r_email" placeholder="E-mail"/></div>
        <div style="margin-top:8px"><input id="r_id" placeholder="ID"/></div>
        <div style="margin-top:8px"><input id="r_pass" type="password" placeholder="Contraseña"/></div>
        <div style="margin-top:12px;display:flex;gap:8px">
          <button id="regBtn" class="btn" style="background:#06b6d4">Crear cuenta</button>
          <button id="regBack" class="btn" style="background:#e5e7eb;color:#111">Volver</button>
        </div>
      </div>

      <div id="adminView" class="hidden">
        <h2>Acceso Administrador</h2>
        <div id="admError" class="small-muted"></div>
        <div style="margin-top:12px"><input id="adm_code" placeholder="Código de Admin"/></div>
        <div style="margin-top:12px;display:flex;gap:8px">
          <button id="admBtn" class="btn" style="background:#7c3aed">Entrar como Admin</button>
          <button id="admBack" class="btn" style="background:#e5e7eb;color:#111">Volver</button>
        </div>
      </div>
    </div>

    <div id="mainArea" class="card hidden" style="margin-top:18px">
      <h2>Secciones</h2>
      <div class="grid cols-3" style="margin-top:12px">
        <div class="section-btn" data-key="tao"> <div style="font-weight:700">🌿 Taoísmo</div><div class="small-muted">Cosmología, Tao, Yin/Yang, energía vital.</div></div>
        <div class="section-btn" data-key="hindu"> <div style="font-weight:700">🕉 Hinduismo</div><div class="small-muted">Dharma, Karma, ciclos cósmicos y deidades.</div></div>
        <div class="section-btn" data-key="jud"> <div style="font-weight:700">✡ Judaísmo</div><div class="small-muted">Génesis, creación en días y tradición hebrea.</div></div>
      </div>

      <div style="margin-top:18px">
        <h3>Comentarios y Opiniones</h3>
        <div class="flex" style="margin-top:8px">
          <div class="stars" id="stars">☆☆☆☆☆</div>
          <div class="small-muted" id="ratingText">0 / 5</div>
        </div>
        <div style="margin-top:8px"><textarea id="commentText" placeholder="Escribe un comentario..."></textarea></div>
        <div style="margin-top:8px;display:flex;gap:8px">
          <button id="sendComment" class="btn">Enviar</button>
          <button id="clearComment" class="btn" style="background:#e5e7eb;color:#111">Borrar</button>
        </div>

        <div id="commentsList" style="margin-top:12px"></div>
      </div>
    </div>

    <div id="adminPanel" class="card hidden" style="margin-top:18px">
      <h3>Panel Admin</h3>
      <div class="flex" style="margin-top:8px">
        <div>Total usuarios: <span id="totalUsers">0</span></div>
      </div>
      <div style="margin-top:12px">
        <div class="admin-list" id="usersList"></div>
      </div>

      <div id="editBlock" class="hidden" style="margin-top:12px">
        <h4>Editar Usuario</h4>
        <div><input id="e_email" placeholder="E-mail"/></div>
        <div style="margin-top:8px"><input id="e_id" placeholder="ID"/></div>
        <div style="margin-top:8px;display:flex;gap:8px"><button id="saveUser" class="btn">Guardar</button><button id="delUser" class="btn" style="background:#ef4444">Borrar</button></div>
      </div>
    </div>

    <div id="profileModal" class="hidden">
      <div class="modal-back">
        <div class="modal">
          <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:8px">
            <div style="font-weight:700">Información Personal</div>
            <div class="close" id="closeProfile">✖</div>
          </div>
          <div>ID: <span id="p_id"></span></div>
          <div style="margin-top:8px">E-mail: <span id="p_email"></span></div>
          <div style="margin-top:8px">
            <input id="p_pass" type="password" placeholder="Nueva contraseña" />
          </div>
          <div style="margin-top:8px">Foto de perfil</div>
          <div class="file-drop" id="fileDrop">Arrastra o haz clic para subir</div>
          <div id="previewWrap" style="margin-top:8px"></div>
          <div style="margin-top:12px;display:flex;gap:8px"><button id="saveProfileBtn" class="btn">Guardar</button><button id="cancelProfile" class="btn" style="background:#e5e7eb;color:#111">Cancelar</button></div>
        </div>
      </div>
    </div>

  </div>

  <script>
    document.addEventListener('DOMContentLoaded', function(){
      const religionInfo = {
        tao: { title: "Taoísmo", subtitle: "Cosmología del Tao", overview: "El Tao es el origen indefinible del universo. Del Vacío surgen Yin y Yang; de su interacción nace el Qi y los Diez Mil Seres.", imgContent: ["El Tao: principio eterno e inmutable.", "Vacío primordial del cual surge todo.", "Yin/Yang: fuerzas opuestas complementarias.", "Qi: energía vital que organiza el cosmos.", "Generación de los Diez Mil Seres (todas las formas)."]},
        hindu: { title: "Hinduismo", subtitle: "Cosmología Védica y Ciclos", overview: "El universo pasa por ciclos de creación, preservación y destrucción. Brahmá crea, Vishnú preserva, Shiva transforma.", imgContent: ["Ciclos del universo: creación, preservación, destrucción.", "Dioses: Brahmá (creador), Vishnú (preservador), Shiva (transformador).", "Reencarnación y karma regulan la vida.", "El universo es cíclico."]},
        jud: { title: "Judaísmo", subtitle: "Génesis y Creación en Seis Días", overview: "Dios crea el mundo en seis días: luz, cielo, tierra, astros, seres vivos y al ser humano.", imgContent: ["Día 1: Luz y oscuridad.", "Día 2: Separación de aguas y cielo.", "Día 3: Tierra firme y vegetación.", "Día 4: Sol, luna y estrellas.", "Día 5: Animales del mar y aves.", "Día 6: Animales terrestres y el ser humano.", "Día 7: Shabat - descanso."]}
      };

      let state = { users: {}, registered: [], current: null, preview: null, comments: [] };

      const $ = (id) => document.getElementById(id);

      function showLogin(){ $('loginView').classList.remove('hidden'); $('registerView').classList.add('hidden'); $('adminView').classList.add('hidden'); }
      function showRegister(){ $('loginView').classList.add('hidden'); $('registerView').classList.remove('hidden'); $('adminView').classList.add('hidden'); }
      function showAdminLogin(){ $('loginView').classList.add('hidden'); $('registerView').classList.add('hidden'); $('adminView').classList.remove('hidden'); }

      function updateWelcome(){
        if(state.current){
          $('welcomeText').textContent = 'Bienvenido:';
          $('welcomeId').textContent = state.current.id;
          $('logoutBtn').classList.remove('hidden');
          $('authArea').classList.add('hidden');
          $('mainArea').classList.remove('hidden');
          if(state.current.id === 'ADMIN'){
            $('adminPanel').classList.remove('hidden');
          } else {
            $('adminPanel').classList.add('hidden');
          }
        } else {
          $('welcomeText').textContent = 'Bienvenido';
          $('welcomeId').textContent = '';
          $('logoutBtn').classList.add('hidden');
          $('authArea').classList.remove('hidden');
          $('mainArea').classList.add('hidden');
          $('adminPanel').classList.add('hidden');
        }
        $('totalUsers').textContent = state.registered.length;
      }

      function renderUsers(){
        const wrap = $('usersList'); wrap.innerHTML = '';
        state.registered.slice().reverse().forEach(u => {
          const el = document.createElement('div'); el.className='list-item';
          el.innerHTML = `<div style="display:flex;justify-content:space-between;align-items:center"><div><strong>${u.id}</strong><div class='small-muted'>${u.email}</div><div class='small-muted'>${u.date}</div></div><div><button data-id='${u.id}' class='editBtn'>Editar</button></div></div>`;
          wrap.appendChild(el);
        });
        document.querySelectorAll('.editBtn').forEach(b=>b.addEventListener('click',e=>{
          const id = e.target.getAttribute('data-id');
          const u = state.registered.find(x=>x.id===id);
          if(u){
            $('editBlock').classList.remove('hidden');
            $('e_email').value = u.email; $('e_id').value = u.id;
            $('saveUser').onclick = ()=>{ saveUserEdit(u.id); };
            $('delUser').onclick = ()=>{ deleteUserId(u.id); };
          }
        }));
      }

      function saveUserEdit(oldId){
        const newEmail = $('e_email').value.trim(); const newId = $('e_id').value.trim();
        if(!newEmail || !newId) return alert('Rellena campos');
        state.registered = state.registered.map(r => r.id===oldId ? {...r,email:newEmail,id:newId} : r);
        if(state.users[oldId]){ const tmp = state.users[oldId]; delete state.users[oldId]; tmp.email=newEmail; tmp.id=newId; state.users[newId]=tmp; }
        $('editBlock').classList.add('hidden'); renderUsers(); updateWelcome();
      }

      function deleteUserId(id){
        if(!confirm('Borrar usuario '+id+'?')) return;
        state.registered = state.registered.filter(r => r.id!==id);
        if(state.users[id]) delete state.users[id];
        $('editBlock').classList.add('hidden'); renderUsers(); updateWelcome();
      }

      function openReligion(key){
        const r = religionInfo[key]; if(!r) return;
        const html = `<!doctype html><html><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>${r.title}</title></head><body style="font-family:Inter,Arial;padding:20px;max-width:900px;margin:0 auto"><h1>${r.title}</h1><h3>${r.subtitle}</h3><p>${r.overview}</p><ul>`+r.imgContent.map(i=>`<li>${i}</li>`).join('')+`</ul></body></html>`;
        let win = null;
        try{ win = window.open('about:blank'); }catch(e){ win = null; }
        if(!win){ const blob = new Blob([html],{type:'text/html'}); const url = URL.createObjectURL(blob); const a = document.createElement('a'); a.href=url; a.target='_blank'; a.rel='noopener'; document.body.appendChild(a); a.click(); a.remove(); setTimeout(()=>URL.revokeObjectURL(url),3000); return; }
        try{ if(win && win.document){ win.document.write(html); win.document.close(); } else { const blob = new Blob([html],{type:'text/html'}); const url = URL.createObjectURL(blob); const a = document.createElement('a'); a.href=url; a.target='_blank'; document.body.appendChild(a); a.click(); a.remove(); setTimeout(()=>URL.revokeObjectURL(url),3000); } }catch(e){ const blob = new Blob([html],{type:'text/html'}); const url = URL.createObjectURL(blob); const a = document.createElement('a'); a.href=url; a.target='_blank'; document.body.appendChild(a); a.click(); a.remove(); setTimeout(()=>URL.revokeObjectURL(url),3000); }
      }

      function attachSectionHandlers(){ document.querySelectorAll('.section-btn').forEach(b=>b.addEventListener('click',()=>openReligion(b.getAttribute('data-key')))); }

      attachSectionHandlers();

      $('showRegister').onclick = showRegister; $('regBack').onclick = showLogin; $('showAdmin').onclick = showAdminLogin; $('admBack').onclick = showLogin;

      $('regBtn').onclick = ()=>{
        const e=$('r_email').value.trim(), id=$('r_id').value.trim(), p=$('r_pass').value;
        if(!e||!id||!p){ $('regError').textContent='Rellena todos los campos'; return; }
        if(state.users[id]){ $('regError').textContent='Ese ID ya existe'; return; }
        const now = new Date().toLocaleString(); state.users[id]={email:e,id,pass:p,photo:null}; state.registered.push({email:e,id,date:now,photo:null}); $('regError').textContent=''; showLogin(); alert('Cuenta creada'); renderUsers();
      };

      $('loginBtn').onclick = ()=>{
        const e=$('email').value.trim(), id=$('userid').value.trim(), p=$('pass').value;
        const u = state.users[id]; if(!u||u.pass!==p||u.email!==e){ $('authError').textContent='Usuario incorrecto. No tienes cuenta: Regístrate.'; return; }
        state.current={id:id,email:e}; $('authError').textContent=''; updateWelcome();
      };

      $('logoutBtn').onclick = ()=>{ state.current=null; updateWelcome(); };

      $('admBtn').onclick = ()=>{
        const c=$('adm_code').value.trim(); if(c!=='27013'){ $('admError').textContent='Código incorrecto'; return; } state.current={id:'ADMIN',email:'admin@system.com'}; updateWelcome();
      };

      $('avatarBtn').onclick = ()=>{ if(!state.current) return; $('p_id').textContent=state.current.id; $('p_email').textContent=state.current.email; $('profileModal').classList.remove('hidden'); };
      $('closeProfile').onclick = ()=>{ $('profileModal').classList.add('hidden'); };
      $('cancelProfile').onclick = ()=>{ $('profileModal').classList.add('hidden'); };

      const fileDrop = $('fileDrop'); if(fileDrop){ fileDrop.addEventListener('dragover',e=>e.preventDefault()); fileDrop.addEventListener('drop',e=>{ e.preventDefault(); const f=e.dataTransfer.files&&e.dataTransfer.files[0]; if(f) handleFile(f); }); fileDrop.addEventListener('click',()=>{ const inp=document.createElement('input'); inp.type='file'; inp.accept='image/*'; inp.onchange=(e)=>{ const f=e.target.files&&e.target.files[0]; if(f) handleFile(f); }; inp.click(); }); }
      function handleFile(file){ const reader=new FileReader(); reader.onload=(ev)=>{ state.preview=ev.target.result; $('previewWrap').innerHTML=`<img src='${state.preview}' style='width:96px;height:96px;border-radius:999px;object-fit:cover'/>`; }; reader.readAsDataURL(file); }

      $('saveProfileBtn').onclick = ()=>{
        const np = $('p_pass').value.trim(); if(state.current && state.current.id !== 'ADMIN'){ const u = state.users[state.current.id]; if(u){ if(np) u.pass=np; if(state.preview) u.photo=state.preview; state.registered = state.registered.map(r=> r.id===u.id? {...r,photo:u.photo}:r); } }
        $('profileModal').classList.add('hidden'); $('previewWrap').innerHTML=''; state.preview=null; $('p_pass').value=''; renderUsers(); updateWelcome();
      };

      const stars = $('stars'); const ratingText = $('ratingText'); let curRating = 0; if(stars){ stars.addEventListener('click', (e)=>{ const rect = stars.getBoundingClientRect(); const x = e.clientX - rect.left; const w = rect.width; const n = Math.ceil((x/w)*5); curRating = Math.max(1, Math.min(5,n)); stars.textContent = '★'.repeat(curRating)+'☆'.repeat(5-curRating); ratingText.textContent = curRating+' / 5'; }); }

      $('sendComment').onclick = ()=>{
        if(!state.current){ alert('Entra o regístrate para comentar.'); return; }
        const text = $('commentText').value.trim(); if(!text){ alert('Escribe un comentario'); return; }
        const now = new Date().toLocaleString(); state.comments.push({id:state.current.id,email:state.current.email,rating:curRating,comment:text,date:now}); $('commentText').value=''; curRating=0; if(stars){ stars.textContent='☆☆☆☆☆'; } ratingText.textContent='0 / 5'; renderComments();
      };
      function renderComments(){ const cwrap=$('commentsList'); cwrap.innerHTML=''; state.comments.slice().reverse().forEach(c=>{ const d=document.createElement('div'); d.className='list-item'; d.innerHTML=`<div style="display:flex;justify-content:space-between"><div><strong>${c.id}</strong><div class='small-muted'>${c.email}</div></div><div class='small-muted'>${c.date}</div></div><div style='margin-top:8px'>${'★'.repeat(c.rating)+'☆'.repeat(5-c.rating)}</div><div style='margin-top:8px'>${c.comment}</div>`; cwrap.appendChild(d); }); }

      function renderUsersAndStats(){ renderUsers(); $('totalUsers').textContent = state.registered.length; }

      showLogin(); updateWelcome(); renderUsersAndStats();
    });
  </script>
</body>
</html>
