import React, { useRef, useState } from "react";

export default function App() {
  const [mode, setMode] = useState("login");
  const [users, setUsers] = useState({});
  const [registeredUsers, setRegisteredUsers] = useState([]);
  const [form, setForm] = useState({ email: "", id: "", pass: "", code: "" });
  const [current, setCurrent] = useState(null);
  const [error, setError] = useState("");

  const [selectedUser, setSelectedUser] = useState(null);
  const [editData, setEditData] = useState({ email: "", id: "" });

  const [activeReligion, setActiveReligion] = useState(null);

  const [showProfile, setShowProfile] = useState(false);
  const [profilePass, setProfilePass] = useState("");
  const [previewPhoto, setPreviewPhoto] = useState(null);
  const fileInputRef = useRef(null);

  const [rating, setRating] = useState(0);
  const [comment, setComment] = useState("");
  const [commentsList, setCommentsList] = useState([]);

  const handleChange = (e) => {
    const { name, value } = e.target;
    setForm((p) => ({ ...p, [name]: value }));
  };

  const resetForm = () => setForm({ email: "", id: "", pass: "", code: "" });

  const register = () => {
    if (!form.email || !form.id || !form.pass) {
      setError("Rellena todos los campos.");
      return;
    }
    if (users[form.id]) {
      setError("Ese ID ya existe.");
      return;
    }
    const newU = { email: form.email, id: form.id, pass: form.pass, photo: null };
    const now = new Date().toLocaleString();
    setUsers((p) => ({ ...p, [form.id]: newU }));
    setRegisteredUsers((p) => [...p, { email: newU.email, id: newU.id, date: now, photo: null }]);
    setMode("login");
    resetForm();
    setError("");
  };

  const login = () => {
    const u = users[form.id];
    if (!u || u.pass !== form.pass || u.email !== form.email) {
      setError("Usuario incorrecto. No tienes cuenta: Regístrate.");
      return;
    }
    setCurrent({ id: u.id, email: u.email });
    resetForm();
    setError("");
  };

  const adminLogin = () => {
    if (form.code !== "27013") {
      setError("Código incorrecto.");
      return;
    }
    setCurrent({ id: "ADMIN", email: "admin@system.com" });
    resetForm();
    setError("");
  };

  const logout = () => {
    setCurrent(null);
    setSelectedUser(null);
    setEditData({ email: "", id: "" });
    setActiveReligion(null);
    setShowProfile(false);
    setProfilePass("");
    setPreviewPhoto(null);
    resetForm();
    setError("");
  };

  const selectUser = (u) => {
    setSelectedUser(u);
    setEditData({ email: u.email, id: u.id });
    setError("");
  };

  const saveEdit = () => {
    if (!selectedUser) return;
    setRegisteredUsers((prev) => prev.map((ru) => (ru.id === selectedUser.id ? { ...ru, email: editData.email, id: editData.id } : ru)));
    setUsers((prev) => {
      const copy = { ...prev };
      const old = copy[selectedUser.id];
      if (old) delete copy[selectedUser.id];
      copy[editData.id] = { email: editData.email, id: editData.id, pass: old ? old.pass : "", photo: old ? old.photo : null };
      return copy;
    });
    setSelectedUser({ ...selectedUser, email: editData.email, id: editData.id });
    setError("");
  };

  const deleteUser = () => {
    if (!selectedUser) return;
    setRegisteredUsers((prev) => prev.filter((ru) => ru.id !== selectedUser.id));
    setUsers((prev) => {
      const copy = { ...prev };
      delete copy[selectedUser.id];
      return copy;
    });
    setSelectedUser(null);
    setEditData({ email: "", id: "" });
    setError("");
  };

  const onFile = (file) => {
    if (!file) return;
    const reader = new FileReader();
    reader.onload = (e) => setPreviewPhoto(e.target.result);
    reader.readAsDataURL(file);
  };

  const onDrop = (e) => {
    e.preventDefault();
    const f = e.dataTransfer.files && e.dataTransfer.files[0];
    if (f) onFile(f);
  };

  const onPick = () => fileInputRef.current && fileInputRef.current.click();
  const handlePicked = (e) => {
    const f = e.target.files && e.target.files[0];
    if (f) onFile(f);
  };

  const saveProfile = () => {
    if (!current || current.id === "ADMIN") {
      setShowProfile(false);
      return;
    }
    setUsers((prev) => {
      const copy = { ...prev };
      const u = copy[current.id];
      if (!u) return prev;
      if (profilePass) u.pass = profilePass;
      if (previewPhoto) u.photo = previewPhoto;
      copy[current.id] = u;
      return copy;
    });
    setRegisteredUsers((prev) => prev.map((ru) => (ru.id === current.id ? { ...ru, photo: previewPhoto ? previewPhoto : ru.photo } : ru)));
    setShowProfile(false);
    setProfilePass("");
  };

  const religionInfo = {
    tao: {
      title: "Taoísmo",
      subtitle: "Cosmología del Tao",
      overview: "El Tao es el origen indefinible del universo. Del Vacío surgen el Yin y Yang, y de su interacción nace la energía vital (Qi) y los Diez Mil Seres.",
      imgContent: [
        "El Tao: principio eterno e inmutable.",
        "Vacío primordial del cual surge todo.",
        "Yin/Yang: fuerzas opuestas complementarias.",
        "Qi: energía vital que organiza el cosmos.",
        "Generación de los Diez Mil Seres (todas las formas existentes).",
      ],
    },
    hindu: {
      title: "Hinduismo",
      subtitle: "Cosmología Védica y Ciclos",
      overview: "El universo pasa por ciclos infinitos de creación, preservación y destrucción. Brahmá crea, Vishnú preserva, Shiva transforma.",
      imgContent: [
        "Ciclos del universo: creación, preservación, destrucción.",
        "Dioses: Brahmá (creador), Vishnú (preservador), Shiva (transformador).",
        "Reencarnación y karma regulan la vida en cada ciclo.",
        "El universo es cíclico y eterno en su repetición.",
      ],
    },
    jud: {
      title: "Judaísmo",
      subtitle: "Génesis y Creación en Seis Días",
      overview: "Dios crea el mundo en seis días: luz, cielo, tierra, astros, seres vivos y finalmente al ser humano.",
      imgContent: [
        "Día 1: Luz y oscuridad.",
        "Día 2: Separación de aguas y cielo.",
        "Día 3: Tierra firme y vegetación.",
        "Día 4: Sol, luna y estrellas.",
        "Día 5: Animales del mar y aves.",
        "Día 6: Animales terrestres y el ser humano.",
        "Día 7: Dios descansa (Shabat).",
      ],
    },
  };

  const openReligionInNewTab = (key) => {
    const r = religionInfo[key];
    if (!r) return;
    const listItems = r.imgContent.map((i) => `<li>${i}</li>`).join("");
    const html = `<!doctype html><html><head><meta charset="utf-8" /><meta name="viewport" content="width=device-width,initial-scale=1" /><title>${r.title}</title></head><body style="font-family:system-ui,Segoe UI,Roboto,Helvetica,Arial,sans-serif;padding:24px;max-width:900px;margin:0 auto;">` +
      `<h1 style="font-size:28px;margin-bottom:6px;">${r.title}</h1>` +
      `<h2 style="color:#666;margin-top:0;margin-bottom:16px;">${r.subtitle}</h2>` +
      `<p style="line-height:1.5;">${r.overview}</p>` +
      `<h3>Detalles</h3><ul>${listItems}</ul>` +
      `</body></html>`;

    let win = null;
    try {
      win = window.open("about:blank", "_blank");
    } catch (e) {
      win = null;
    }

    if (!win) {
      const blob = new Blob([html], { type: "text/html" });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.target = "_blank";
      a.rel = "noopener";
      document.body.appendChild(a);
      a.click();
      a.remove();
      setTimeout(() => URL.revokeObjectURL(url), 5000);
      return;
    }

    try {
      win.document.write(html);
      win.document.close();
    } catch (e) {
      const blob = new Blob([html], { type: "text/html" });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.target = "_blank";
      a.rel = "noopener";
      document.body.appendChild(a);
      a.click();
      a.remove();
      setTimeout(() => URL.revokeObjectURL(url), 5000);
    }
  };

  const submitComment = () => {
    if (!current) {
      setError("Debes iniciar sesión para comentar.");
      return;
    }
    if (!comment) {
      setError("Escribe un comentario.");
      return;
    }
    const now = new Date().toLocaleString();
    setCommentsList((p) => [...p, { id: current.id, email: current.email, rating, comment, date: now }]);
    setComment("");
    setRating(0);
    setError("");
  };

  if (!current) {
    if (mode === "login") {
      return (
        <div className="w-screen h-screen flex items-center justify-center bg-gray-50 p-6">
          <div className="bg-white shadow-2xl rounded-3xl p-8 w-full max-w-md">
            <h2 className="text-2xl font-extrabold mb-4">Iniciar Sesión</h2>
            {error && <div className="text-red-600 mb-3">{error}</div>}

            <input name="email" onChange={handleChange} value={form.email} placeholder="E-mail" className="w-full p-3 rounded-xl border mb-3" />
            <input name="id" onChange={handleChange} value={form.id} placeholder="ID" className="w-full p-3 rounded-xl border mb-3" />
            <input name="pass" type="password" onChange={handleChange} value={form.pass} placeholder="Contraseña" className="w-full p-3 rounded-xl border mb-3" />

            <button onClick={login} className="mt-2 w-full py-3 rounded-xl font-semibold bg-gradient-to-r from-blue-500 to-indigo-500 text-white">Entrar</button>

            <div className="mt-4 grid grid-cols-2 gap-3">
              <div className="p-3 rounded-xl border text-center cursor-pointer hover:bg-gray-100" onClick={() => { setMode("register"); setError(""); }}>Regístrate</div>
              <div className="p-3 rounded-xl border text-center cursor-pointer bg-yellow-50 hover:bg-yellow-100" onClick={() => { setMode("admin"); setError(""); }}>Soy Admin</div>
            </div>
          </div>
        </div>
      );
    }

    if (mode === "register") {
      return (
        <div className="w-screen h-screen flex items-center justify-center bg-gray-50 p-6">
          <div className="bg-white shadow-2xl rounded-3xl p-8 w-full max-w-md">
            <h2 className="text-2xl font-extrabold mb-4">Crear cuenta</h2>
            {error && <div className="text-red-600 mb-3">{error}</div>}

            <input name="email" onChange={handleChange} value={form.email} placeholder="E-mail" className="w-full p-3 rounded-xl border mb-3" />
            <input name="id" onChange={handleChange} value={form.id} placeholder="ID" className="w-full p-3 rounded-xl border mb-3" />
            <input name="pass" type="password" onChange={handleChange} value={form.pass} placeholder="Contraseña" className="w-full p-3 rounded-xl border mb-3" />

            <button onClick={register} className="mt-2 w-full py-3 rounded-xl font-semibold bg-gradient-to-r from-green-400 to-green-300 text-white">Crear cuenta</button>

            <div className="mt-4 text-center cursor-pointer underline" onClick={() => { setMode("login"); setError(""); }}>Volver</div>
          </div>
        </div>
      );
    }

    if (mode === "admin") {
      return (
        <div className="w-screen h-screen flex items-center justify-center bg-gray-50 p-6">
          <div className="bg-white shadow-2xl rounded-3xl p-8 w-full max-w-md">
            <h2 className="text-2xl font-extrabold mb-4">Acceso Administrador</h2>
            {error && <div className="text-red-600 mb-3">{error}</div>}

            <input name="code" onChange={handleChange} value={form.code} placeholder="Código de Admin" className="w-full p-3 rounded-xl border mb-3" />
            <button onClick={adminLogin} className="mt-2 w-full py-3 rounded-xl font-semibold bg-gradient-to-r from-purple-600 to-purple-500 text-white">Entrar como Admin</button>

            <div className="mt-4 text-center cursor-pointer underline" onClick={() => setMode("login")}>Volver</div>
          </div>
        </div>
      );
    }
  }

  if (current && current.id === "ADMIN") {
    return (
      <div className="w-screen h-screen bg-gray-50 p-6 flex gap-6">
        <div className="flex-1">
          <div className="flex items-center gap-4 mb-6">
            <button className="p-3 rounded-full bg-white shadow" onClick={() => setShowProfile(true)}>👤</button>
            <h1 className="text-xl font-semibold">Bienvenido Admin</h1>
            <button onClick={logout} className="ml-auto bg-red-500 text-white px-3 py-1 rounded-xl">Salir</button>
          </div>

          <div className="bg-white shadow rounded-2xl p-6">
            <h2 className="text-2xl font-bold mb-2">Panel de Administración</h2>

            <div className="bg-gray-50 p-4 rounded-xl shadow mb-4">
              <h3 className="text-xl font-semibold mb-2">📊 Estadísticas</h3>
              <p>Total de usuarios registrados: {registeredUsers.length}</p>
            </div>

            {selectedUser && (
              <div className="bg-white p-4 border rounded-xl shadow">
                <h3 className="text-xl font-semibold mb-2">Editar Usuario</h3>
                <input value={editData.email} onChange={(e) => setEditData((p) => ({ ...p, email: e.target.value }))} className="w-full p-2 border rounded-xl mb-2" />
                <input value={editData.id} onChange={(e) => setEditData((p) => ({ ...p, id: e.target.value }))} className="w-full p-2 border rounded-xl mb-2" />
                <div className="flex gap-2">
                  <button onClick={saveEdit} className="bg-blue-500 text-white px-3 py-1 rounded-xl mr-2">Guardar</button>
                  <button onClick={deleteUser} className="bg-red-500 text-white px-3 py-1 rounded-xl">Borrar Usuario</button>
                </div>
              </div>
            )}
          </div>
        </div>

        <div className="w-80 bg-white shadow rounded-2xl p-6 overflow-y-auto">
          <h2 className="text-xl font-semibold mb-4">Usuarios Registrados</h2>
          <ul className="space-y-3">
            {registeredUsers.length === 0 && <li className="text-gray-500">No hay usuarios registrados</li>}
            {registeredUsers.map((u, i) => (
              <li key={i} className="p-3 rounded-xl border hover:bg-gray-100 cursor-pointer" onClick={() => selectUser(u)}>
                <div className="flex items-center gap-2">
                  <div className="w-8 h-8 rounded-full bg-gray-100 overflow-hidden flex items-center justify-center">
                    {u.photo ? <img src={u.photo} alt="avatar" className="w-full h-full object-cover" /> : <span className="text-xs text-gray-500">{u.id.charAt(0)}</span>}
                  </div>
                  <div>
                    <div className="font-medium">{u.id}</div>
                    <div className="text-xs text-gray-500">{u.email}</div>
                    <div className="text-xs text-gray-400">{u.date}</div>
                  </div>
                </div>
              </li>
            ))}
          </ul>
        </div>
      </div>
    );
  }

  return (
    <div className="w-screen h-screen bg-gray-50 p-6 overflow-auto">
      <div className="flex items-center gap-4 mb-6">
        <button className="p-3 rounded-full bg-white shadow" onClick={() => setShowProfile(true)}>
          {current && current.id && (
            <div className="w-8 h-8 rounded-full overflow-hidden">
              {users[current.id] && users[current.id].photo ? (
                <img src={users[current.id].photo} alt="me" className="w-full h-full object-cover" />
              ) : (
                <div className="w-full h-full flex items-center justify-center text-sm">👤</div>
              )}
            </div>
          )}
        </button>
        <h1 className="text-xl font-semibold">Bienvenido: {current && current.id}</h1>
        <button onClick={logout} className="ml-auto bg-red-500 text-white px-3 py-1 rounded-xl">Salir</button>
      </div>

      <div className="bg-white shadow rounded-2xl p-6 max-w-4xl mx-auto">
        <h2 className="text-3xl font-bold mb-6">Secciones</h2>

        <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
          <button onClick={() => openReligionInNewTab('tao')} className="p-6 rounded-2xl shadow bg-white hover:shadow-lg transition text-left border">
            <div className="text-2xl font-bold mb-1">🌿 Taoísmo</div>
            <div className="text-sm text-gray-600">Cosmología, Tao, Yin/Yang, energía vital.</div>
          </button>

          <button onClick={() => openReligionInNewTab('hindu')} className="p-6 rounded-2xl shadow bg-white hover:shadow-lg transition text-left border">
            <div className="text-2xl font-bold mb-1">🕉 Hinduismo</div>
            <div className="text-sm text-gray-600">Dharma, Karma, ciclos cósmicos y deidades.</div>
          </button>

          <button onClick={() => openReligionInNewTab('jud')} className="p-6 rounded-2xl shadow bg-white hover:shadow-lg transition text-left border">
            <div className="text-2xl font-bold mb-1">✡ Judaísmo</div>
            <div className="text-sm text-gray-600">Génesis, creación en días y tradición hebrea.</div>
          </button>
        </div>
      </div>

      <div className="bg-white shadow rounded-2xl p-6 max-w-4xl mx-auto mt-6">
        <h2 className="text-3xl font-bold mb-4">Comentarios y Opiniones</h2>
        <div className="mb-4 flex items-center gap-2">
          <span className="font-semibold">Tu puntuación:</span>
          <div className="flex gap-1 text-2xl cursor-pointer select-none">
            {[1, 2, 3, 4, 5].map((n) => (
              <span key={n} onClick={() => setRating(n)} style={{ opacity: rating >= n ? 1 : 0.3 }}>⭐</span>
            ))}
          </div>
        </div>
        <textarea value={comment} onChange={(e) => setComment(e.target.value)} placeholder="Escribe un comentario..." className="w-full p-3 border rounded-xl h-28 mb-3" />
        <div className="flex gap-2">
          <button onClick={submitComment} className="px-4 py-2 rounded-xl bg-blue-500 text-white">Enviar</button>
          <button onClick={() => { setComment(""); setRating(0); }} className="px-4 py-2 rounded-xl bg-gray-200">Borrar</button>
        </div>

        <div className="mt-6 space-y-3">
          {commentsList.length === 0 && <div className="text-gray-500">No hay comentarios aún.</div>}
          {commentsList.map((c, i) => (
            <div key={i} className="p-3 border rounded-xl">
              <div className="flex items-center justify-between">
                <div className="font-semibold">{c.id} <span className="text-xs text-gray-500">{c.email}</span></div>
                <div className="text-yellow-500">{Array.from({ length: c.rating }).map((_, j) => "⭐").join("")}</div>
              </div>
              <div className="text-sm text-gray-700 mt-2">{c.comment}</div>
              <div className="text-xs text-gray-400 mt-2">{c.date}</div>
            </div>
          ))}
        </div>
      </div>

      {activeReligion && (
        <div className="fixed inset-0 bg-black bg-opacity-40 flex items-center justify-center p-4 z-50">
          <div className="bg-white p-6 rounded-2xl shadow-xl w-full max-w-xl relative">
            <button className="absolute top-3 right-3 text-xl" onClick={() => setActiveReligion(null)}>✖</button>
            <h2 className="text-3xl font-bold mb-2">{activeReligion.title}</h2>
            <h3 className="text-xl text-gray-600 mb-4">{activeReligion.subtitle}</h3>
            <p className="mb-4 text-gray-700 leading-relaxed">{activeReligion.overview}</p>
            <ul className="list-disc pl-5 space-y-1 text-gray-700">
              {activeReligion.imgContent.map((t,i)=>(<li key={i}>{t}</li>))}
            </ul>
          </div>
        </div>
      )}

      {showProfile && current && (
        <div className="fixed inset-0 bg-black bg-opacity-40 flex items-center justify-center p-4 z-50">
          <div className="bg-white p-6 rounded-2xl shadow-xl w-full max-w-md relative">
            <button className="absolute top-3 right-3 text-xl" onClick={() => setShowProfile(false)}>✖</button>
            <h2 className="text-2xl font-bold mb-4">Información Personal</h2>

            <div className="mb-3">
              <label className="font-semibold">ID:</label>
              <div className="p-2 bg-gray-100 rounded">{current.id}</div>
            </div>

            <div className="mb-3">
              <label className="font-semibold">E-mail:</label>
              <div className="p-2 bg-gray-100 rounded">{current.email}</div>
            </div>

            <div className="mb-3">
              <label className="font-semibold">Nueva contraseña:</label>
              <input value={profilePass} onChange={(e) => setProfilePass(e.target.value)} type="password" className="w-full p-2 border rounded" placeholder="Cambiar contraseña" />
            </div>

            <div className="mb-4">
              <label className="font-semibold">Foto de perfil</label>

              <div
                onDragOver={(e) => e.preventDefault()}
                onDrop={onDrop}
                className="mt-2 border-2 border-dashed border-gray-300 rounded-xl p-4 flex flex-col items-center justify-center cursor-pointer"
                onClick={onPick}
              >
                {previewPhoto ? (
                  <div className="flex flex-col items-center">
                    <img src={previewPhoto} alt="preview" className="w-28 h-28 rounded-full object-cover mb-3 shadow" />
                    <div className="text-sm text-gray-600 mb-2">Vista previa</div>
                    <div className="flex gap-2">
                      <button onClick={(e) => { e.stopPropagation(); setPreviewPhoto(null); if (fileInputRef.current) fileInputRef.current.value = null; }} className="px-3 py-1 rounded-xl bg-gray-200">Eliminar</button>
                      <button onClick={(e) => { e.stopPropagation(); if (fileInputRef.current) fileInputRef.current.click(); }} className="px-3 py-1 rounded-xl bg-blue-500 text-white">Cambiar</button>
                    </div>
                  </div>
                ) : (
                  <div className="text-center text-gray-500">
                    <div className="text-4xl">📤</div>
                    <div className="mt-2 font-medium">Arrastra la imagen o haz clic para subir</div>
                    <div className="text-sm text-gray-400 mt-1">JPEG, PNG. Máx 5MB</div>
                  </div>
                )}
                <input ref={fileInputRef} type="file" accept="image/*" className="hidden" onChange={handlePicked} />
              </div>
            </div>

            <div className="flex gap-2">
              <button onClick={saveProfile} className="w-full bg-blue-500 text-white py-2 rounded-xl">Guardar Cambios</button>
              <button onClick={() => setShowProfile(false)} className="w-full bg-gray-200 py-2 rounded-xl">Cancelar</button>
            </div>
          </div>
        </div>
      )}

    </div>
  );
}
