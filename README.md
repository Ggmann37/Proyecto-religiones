<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Secciones Modernistas</title>

<!-- Iconos -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">

<style>
    body {
        font-family: 'Segoe UI', sans-serif;
        background: linear-gradient(135deg, #dfe9f3, #ffffff);
        padding: 40px;
    }

    h1 {
        text-align: center;
        margin-bottom: 40px;
        font-size: 38px;
        letter-spacing: 1px;
    }

    .religion-list {
        max-width: 850px;
        margin: auto;
    }

    .item {
        background: rgba(255, 255, 255, 0.65);
        backdrop-filter: blur(12px);
        padding: 20px;
        border-radius: 18px;
        margin-bottom: 18px;
        border: 1px solid rgba(255, 255, 255, 0.4);
        box-shadow: 0 6px 20px rgba(0,0,0,0.08);
        transition: all 0.35s ease;
        cursor: pointer;
        position: relative;
    }

    .item:hover {
        transform: scale(1.02);
        box-shadow: 0 8px 20px rgba(0,0,0,0.12);
    }

    .item h2 {
        display: flex;
        justify-content: space-between;
        font-size: 24px;
        margin: 0;
    }

    .item h2 i {
        transition: transform 0.3s ease;
    }

    .item.open h2 i {
        transform: rotate(180deg);
    }

    .info {
        max-height: 0;
        overflow: hidden;
        opacity: 0;
        transition: all 0.55s ease;
        padding-left: 5px;
    }

    .item.open .info {
        max-height: 600px;
        opacity: 1;
        margin-top: 15px;
    }

    .info section {
        background: white;
        padding: 15px 20px;
        border-radius: 14px;
        margin-bottom: 14px;
        box-shadow: 0 3px 12px rgba(0,0,0,0.06);
    }

    .info h3 {
        margin-top: 0;
    }
</style>
</head>

<body>

<h1>Secciones</h1>

<div class="religion-list">

    <!-- Cristianismo -->
    <div class="item">
        <h2>Cristianismo <i class="fa-solid fa-chevron-down"></i></h2>
        <div class="info">
            <section>
                <h3>Origen</h3>
                <p>Nació en el siglo I siguiendo las enseñanzas de Jesucristo.</p>
            </section>

            <section>
                <h3>Creación</h3>
                <p>Dios creó el universo en seis días según el Génesis.</p>
            </section>

            <section>
                <h3>Creencias principales</h3>
                <p>Fe en Cristo, vida eterna, resurrección y salvación.</p>
            </section>
        </div>
    </div>

    <!-- Islam -->
    <div class="item">
        <h2>Islam <i class="fa-solid fa-chevron-down"></i></h2>
        <div class="info">
            <section>
                <h3>Origen</h3>
                <p>Fundado por Mahoma en el siglo VII en Arabia.</p>
            </section>

            <section>
                <h3>Creación</h3>
                <p>Alá creó el universo y gobierna sobre él.</p>
            </section>

            <section>
                <h3>Creencias principales</h3>
                <p>Un solo Dios, el Corán y los Cinco Pilares del Islam.</p>
            </section>
        </div>
    </div>

    <!-- Hinduismo -->
    <div class="item">
        <h2>Hinduismo <i class="fa-solid fa-chevron-down"></i></h2>
        <div class="info">
            <section>
                <h3>Origen</h3>
                <p>Antigua tradición nacida en India hace más de 4000 años.</p>
            </section>

            <section>
                <h3>Creación</h3>
                <p>Brahma crea el universo en ciclos infinitos.</p>
            </section>

            <section>
                <h3>Creencias principales</h3>
                <p>Karma, reencarnación, dharma y múltiples deidades.</p>
            </section>
        </div>
    </div>

    <!-- Budismo -->
    <div class="item">
        <h2>Budismo <i class="fa-solid fa-chevron-down"></i></h2>
        <div class="info">
            <section>
                <h3>Origen</h3>
                <p>Fundado por Siddhartha Gautama (Buda) en el siglo V a.C.</p>
            </section>

            <section>
                <h3>Creación</h3>
                <p>No existe un creador; el universo sigue ciclos naturales.</p>
            </section>

            <section>
                <h3>Creencias principales</h3>
                <p>Las Cuatro Nobles Verdades y el Óctuple Sendero.</p>
            </section>
        </div>
    </div>

</div>

<script>
    const items = document.querySelectorAll('.item');

    items.forEach(item => {
        item.addEventListener('click', () => {
            items.forEach(i => {
                if (i !== item) i.classList.remove('open');
            });
            item.classList.toggle('open');
        });
    });
</script>

</body>
</html>
