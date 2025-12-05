<template>
  <div class="page-wrapper">
    <div class="main-content">
      <div class="legal-container">
        <h1>MENTIONS LÉGALES</h1>

        <p>Conformément aux dispositions de la loi n° 2004-575 du 21 juin 2004 pour la confiance en l'économie numérique, il est précisé aux utilisateurs du site god-save-biscotte l'identité des différents intervenants dans le cadre de sa réalisation et de son suivi.</p>

        <h2>Edition du site</h2>
        <p>
          Le présent site, accessible à l'URL www.god-save-biscotte.jrcan.dev (le « Site »), est édité par :<br>
          L'association Jrcandev, enregistrée auprès de la préfecture/sous-préfecture de 62 - Sous-Préfecture Calais sous le numéro W626007087, ayant son siège situé à 19 rue Louis-David, représentée par Julien Behani dûment habilité(e).
        </p>

        <h2>Hébergement</h2>
        <p>Le Site est hébergé par la société Jrcandev, situé 19 rue Louis-David, (contact téléphonique ou email : +33321190600).</p>

        <h2>Directeur de publication</h2>
        <p>Le Directeur de la publication du Site est Julien Behani.</p>

        <h2>Nous contacter</h2>
        <ul>
          <li><strong>Par téléphone :</strong> +33321190600</li>
          <li><strong>Par email :</strong> remi.synave@univ-littoral.fr</li>
          <li><strong>Par courrier :</strong> 19 rue Louis-David 62100 Calais</li>
        </ul>

        <p class="legal-footer"><em>Génération des mentions légales par Legalstart.</em></p>
      </div>
    </div>
    <ChatbotComponent />
    <FooterComponent />
  </div>
</template>

<style scoped>
.page-wrapper {
  padding: 2rem;
  min-height: 100vh;
}

.legal-container {
  max-width: 800px;
  margin: 2rem auto;
  padding: 2rem;
  background-color: #ffffff;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.05);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  color: #333;
  text-align: left;
  line-height: 1.6;
}

.legal-container h1, .legal-container h2 {
  color: #3e2723;
  border-bottom: 2px solid #d7ccc8;
  padding-bottom: 0.5rem;
  margin-bottom: 1rem;
}

.legal-container ul {
  list-style-position: inside;
  padding-left: 0;
}
.legal-footer {
  margin-top: 2rem;
  text-align: center;
  font-size: 0.9rem;
  color: #777;
}
</style>