# <h1>🏠 Conception 👷🏻‍♂️</h1>

Créer un site en React nécessite de résoudre plusieurs problématiques courantes comme l'envoi d'e-mails sans back-end et l'optimisation du SEO. <br>
Voici une explication détaillée pour ces deux aspects :

## 1. 📧 Envoi d'e-mails sans back-end avec EmailJS

### Problématique
Dans une application front-end pure, sans serveur back-end, il peut être difficile de gérer l'envoi d'e-mails. Traditionnellement, cela nécessite un serveur pour configurer un service SMTP ou utiliser une API tierce.

### Solution : Utilisation d'EmailJS
EmailJS est un service qui permet d'envoyer des e-mails directement depuis une application front-end sans besoin de serveur. Il s'intègre avec des services comme Gmail, Outlook, et d'autres, tout en assurant la sécurité via une clé API.

### Mise en œuvre :

####    . Installation d'EmailJS Ajoutez EmailJS à votre projet avec NPM :
       npm install emailjs-com

####    . Configuration
        ◦ Créez un compte sur EmailJS.
        ◦ Configurez un service de messagerie (ex : Gmail).
        ◦ Créez un modèle d'e-mail avec les champs nécessaires (par ex. : from_name, message, etc.).
        ◦ Récupérez votre User ID et les identifiants de service et de modèle.

####    . Code d'intégration - Exemple de code pour envoyer un e-mail :

    import React, { useState } from "react";
    import emailjs from "emailjs-com";
    
    const ContactForm = () => {
      const [formData, setFormData] = useState({
        name: "",
        email: "",
        message: ""
      });
    
      const handleChange = (e) => {
        setFormData({ ...formData, [e.target.name]: e.target.value });
      };
    
      const handleSubmit = (e) => {
        e.preventDefault();
        emailjs.send(
          "your_service_id", // Remplacez par votre Service ID
          "your_template_id", // Remplacez par votre Template ID
          formData,
          "your_user_id" // Remplacez par votre User ID
        )
        .then((response) => {
          console.log("Email envoyé avec succès !", response.status, response.text);
        })
        .catch((err) => {
          console.error("Erreur lors de l'envoi :", err);
        });
      };
    
      return (
        <form onSubmit={handleSubmit}>
          <input type="text" name="name" placeholder="Votre nom" onChange={handleChange} required />
          <input type="email" name="email" placeholder="Votre e-mail" onChange={handleChange} required />
          <textarea name="message" placeholder="Votre message" onChange={handleChange} required />
          <button type="submit">Envoyer</button>
        </form>
      );
    };
    export default ContactForm;

## 🔒 2. Sécurisation des données

### Yup et formik

<b>Yup</b> est un constructeur de schéma pour l'analyse et la validation des valeurs pendant l'exécution. <br />
- Définissez un schéma, transformez une valeur pour qu'elle corresponde, affirmez la structure d'une valeur existante, ou les deux.<br /> 
- Les schémas Yup sont extrêmement expressifs et permettent de modéliser des validations complexes et interdépendantes, ainsi que des transformations de valeurs.
- Schéma pour exclure les caratères < et > :<br />
``` 
text: Yup.string()
    .matches(/^[^<>]*$/, "Les caractères '<' et '>' sont interdits")
    .required("Ce champ est requis"),
``` 
- Schéma pour contrôler un numéro de téléphone français sans espace en version national :<br />
``` 
phone: Yup.string()
    .matches(/^0[1-9]\d{8}$/, "Le numéro doit avoir 10 chiffres et commencer par 0")
    .required("Le numéro de téléphone est requis"),
```
La bibliothèque <b>Formik</b> est une bibliothèque populaire de gestion de formulaires pour React.<br />
- Formik est  conçu pour gérer des formulaires avec une validation complexe ou simple.<br />
- Formik supporte la validation synchrone et asynchrone au niveau du formulaire et du champ.<br /> 
- Le hook personnalisé useFormik aide à simplifier le processus de création et de gestion de formulaires dans les applications React en gérant l'état du formulaire, la validation et la soumission du formulaire.<br />

## 3. 💡 Résolution des problèmes de SEO dans React

React, en tant que framework SPA (Single Page Application), peut poser des défis pour le SEO car la plupart des moteurs de recherche ont des difficultés à indexer les contenus générés dynamiquement.

### Solution 1 : Utiliser React Helmet

React Helmet est une bibliothèque permettant de gérer dynamiquement les balises <head> (titre, meta descriptions, etc.) pour chaque page.

Mise en œuvre avec React Helmet :

####    1. Installation Installez React Helmet :
       npm install react-helmet-async

####    2. Utilisation dans un composant Exemple d'utilisation :

    import React from "react";
    import { Helmet } from "react-helmet-async";
    
    const HomePage = () => {
      return (
        <div>
          <Helmet>
            <title>Accueil - Mon Site React</title>
            <meta name="description" content="Bienvenue sur mon site React optimisé pour le SEO." />
          </Helmet>
          <h1>Bienvenue</h1>
          <p>Ceci est la page d'accueil.</p>
        </div>
      );
    };
    
    export default HomePage;

### Solution 2 : Paramétrer le fichier .htaccess

Mon site React est déployé sur un serveur Apache, il est essentiel de configurer un fichier .htaccess pour gérer les routes et permettre aux moteurs de recherche d'accéder correctement aux pages.

#####    1. Modification du fichier .htaccess 
Modifié le fichier .htaccess à la racine de mon projet build.

#####    2. Configuration typique Ajoutez les lignes suivantes dans le fichier .htaccess :
Ajout des lignes suivantes dans le fichier .htaccess : 

    RewriteRule ^Devis/?$  / [NC,L]
    RewriteRule ^TraitementToiture/?$  / [NC,L]
    RewriteRule ^Couverture/?$  / [NC,L]
    RewriteRule ^RenovationFacade/?$  / [NC,L]
    RewriteRule ^GouttiereZinguerie/?$  / [NC,L]
    RewriteRule ^Realisations/?$  / [NC,L]
    RewriteRule ^Maintenance/?$  / [NC,L]
    ErrorDocument 404 /

Cela garantit que toutes les routes sont redirigées vers index.html pour que React gère le routage.

## 4. 🛠️ Sécurité

J'ai activé HTTPS sur mon hébergeur LWS, mon site web est bien configuré pour utiliser HTTPS de manière optimale. LWS fournit une interface simple pour activer un certificat SSL/TLS. <br>
Cela garantit que les données échangées sont chiffrées, ce qui protège contre les interceptions ou attaques potentielles.

### Vérifier l'activation du certificat SSL

Vérification du fichier .htaccess situé à la racine du site avec la présence des lignes suivantes:
 
    RewriteEngine On <br />
    RewriteCond %{HTTPS} off <br />
    RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301] <br />

Cela redirige tout le trafic HTTP vers HTTPS.

## 5. 📝 Avantages de ces solutions
    • React Helmet permet une gestion fine des balises meta pour chaque page, crucial pour le SEO.
    • .htaccess assure une redirection correcte, empêchant les erreurs 404 lors du rechargement ou des accès directs.

## 6. 🎯 Conclusion
En combinant EmailJS pour l'envoi d'e-mails sans back-end et React Helmet avec un fichier .htaccess bien configuré pour le SEO, 
vous obtenez une application React performante et conviviale pour les utilisateurs comme pour les moteurs de recherche.<br />
Mon site web fonctionne de manière optimale en HTTPS sur LWS. 