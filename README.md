# My-coaching-site
Site to prepare a call with me
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Coach Développement Personnel - Réservation d'appel</title>
    <style>
        * { 
            margin: 0; 
            padding: 0; 
            box-sizing: border-box; 
        }
        
        body { 
            font-family: 'Arial', sans-serif; 
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 500px;
            margin: 50px auto;
            background: white;
            padding: 40px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            display: none; /* Toutes les pages cachées par défaut */
        }
        
        .page-active {
            display: block; /* Seule la page active est visible */
        }
        
        h1 {
            color: #333;
            text-align: center;
            margin-bottom: 30px;
            font-size: 24px;
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        label {
            display: block;
            margin-bottom: 5px;
            color: #555;
            font-weight: bold;
        }
        
        input, textarea {
            width: 100%;
            padding: 12px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
        }
        
        textarea {
            height: 150px;
            resize: vertical;
        }
        
        .button-group {
            display: flex;
            gap: 10px;
            margin-top: 20px;
        }
        
        button {
            flex: 1;
            padding: 15px;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            cursor: pointer;
        }
        
        .btn-primary {
            background: #667eea;
            color: white;
        }
        
        .btn-secondary {
            background: #6c757d;
            color: white;
        }
        
        .btn-success {
            background: #28a745;
            color: white;
        }
        
        button:hover {
            opacity: 0.9;
        }
        
        button:disabled {
            background: #ccc;
            cursor: not-allowed;
        }
        
        .creneau-option {
            border: 2px solid #ddd;
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 10px;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .creneau-option:hover {
            border-color: #667eea;
            background: #f8f9fa;
        }
        
        .creneau-option.selected {
            border-color: #667eea;
            background: #667eea;
            color: white;
        }
        
        .success-icon {
            font-size: 60px;
            color: #28a745;
            margin-bottom: 20px;
            text-align: center;
        }
        
        .recap {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 8px;
            margin: 20px 0;
            text-align: left;
        }
        
        .recap p {
            margin: 10px 0;
        }
    </style>
</head>
<body>
    <!-- PAGE 1 - Formulaire de contact -->
    <div id="page1" class="container page-active">
        <h1>📞 Réservez votre appel découverte de 30min</h1>
        <form id="form1">
            <div class="form-group">
                <label for="nom">Nom :</label>
                <input type="text" id="nom" name="nom" required>
            </div>
            <div class="form-group">
                <label for="prenom">Prénom :</label>
                <input type="text" id="prenom" name="prenom" required>
            </div>
            <div class="form-group">
                <label for="telephone">Numéro de téléphone :</label>
                <input type="tel" id="telephone" name="telephone" required>
            </div>
            <button type="button" class="btn-primary" onclick="changerPage(2)">Continuer</button>
        </form>
    </div>

    <!-- PAGE 2 - Raison de l'appel -->
    <div id="page2" class="container">
        <h1>💬 Quel est la raison de votre appel ?</h1>
        <textarea id="raison" placeholder="Décrivez brièvement ce qui vous amène à réserver cet appel..."></textarea>
        <div class="button-group">
            <button class="btn-secondary" onclick="changerPage(1)">Précédent</button>
            <button class="btn-primary" onclick="changerPage(3)">Continuer</button>
        </div>
    </div>

    <!-- PAGE 3 - Choix du créneau -->
    <div id="page3" class="container">
        <h1>📅 Choisissez votre créneau</h1>
        
        <div id="creneaux-container">
            <div class="creneau-option" data-creneau="lundi-10h">
                <strong>Lundi</strong> - 10h00
            </div>
            <div class="creneau-option" data-creneau="mardi-14h">
                <strong>Mardi</strong> - 14h00
            </div>
            <div class="creneau-option" data-creneau="mercredi-16h">
                <strong>Mercredi</strong> - 16h00
            </div>
            <div class="creneau-option" data-creneau="jeudi-11h">
                <strong>Jeudi</strong> - 11h00
            </div>
        </div>

        <div class="button-group">
            <button class="btn-secondary" onclick="changerPage(2)">Précédent</button>
            <button class="btn-success" id="btnConfirmer" onclick="changerPage(4)" disabled>Confirmer</button>
        </div>
    </div>

    <!-- PAGE 4 - Confirmation -->
    <div id="page4" class="container">
        <div class="success-icon">✅</div>
        <h1>Félicitations !</h1>
        <p>Votre réservation a été enregistrée avec succès.</p>
        
        <div class="recap" id="recap-reservation">
            <!-- Récapitulatif rempli par JavaScript -->
        </div>
        
        <p><strong>Vous recevrez un message de confirmation dans les prochaines 24h.</strong></p>
        <p>Merci de votre confiance !</p>
        
        <div class="button-group">
            <button class="btn-primary" onclick="reinitialiserFormulaire()">Nouvelle réservation</button>
        </div>
    </div>

    <script>
        // Variables pour stocker les données
        let donneesReservation = {
            nom: '',
            prenom: '',
            telephone: '',
            raison: '',
            creneau: ''
        };

        let creneauSelectionne = null;

        // Fonction pour changer de page
        function changerPage(pageDestination) {
            // Valider les données avant de passer à la page suivante
            if (pageDestination === 2) {
                if (!validerPage1()) return;
            } else if (pageDestination === 3) {
                if (!validerPage2()) return;
            } else if (pageDestination === 4) {
                if (!validerPage3()) return;
                afficherRecapitulatif();
            }

            // Cacher toutes les pages
            document.querySelectorAll('.container').forEach(page => {
                page.classList.remove('page-active');
            });

            // Afficher la page demandée
            document.getElementById('page' + pageDestination).classList.add('page-active');
        }

        // Validation de la page 1
        function validerPage1() {
            const nom = document.getElementById('nom').value;
            const prenom = document.getElementById('prenom').value;
            const telephone = document.getElementById('telephone').value;
            
            if (!nom || !prenom || !telephone) {
                alert('Veuillez remplir tous les champs');
                return false;
            }
            
            // Sauvegarder les données
            donneesReservation.nom = nom;
            donneesReservation.prenom = prenom;
            donneesReservation.telephone = telephone;
            
            return true;
        }

        // Validation de la page 2
        function validerPage2() {
            const raison = document.getElementById('raison').value;
            
            if (!raison) {
                alert('Veuillez décrire la raison de votre appel');
                return false;
            }
            
            // Sauvegarder les données
            donneesReservation.raison = raison;
            
            return true;
        }

        // Validation de la page 3
        function validerPage3() {
            if (!creneauSelectionne) {
                alert('Veuillez sélectionner un créneau');
                return false;
            }
            
            // Sauvegarder les données
            donneesReservation.creneau = creneauSelectionne;
            
            return true;
        }

        // Afficher le récapitulatif
        function afficherRecapitulatif() {
            const recap = document.getElementById('recap-reservation');
            recap.innerHTML = `
                <p><strong>Nom :</strong> ${donneesReservation.nom} ${donneesReservation.prenom}</p>
                <p><strong>Téléphone :</strong> ${donneesReservation.telephone}</p>
                <p><strong>Raison :</strong> ${donneesReservation.raison}</p>
                <p><strong>Créneau choisi :</strong> ${donneesReservation.creneau}</p>
            `;
            
            // Ici vous pourriez ajouter l'envoi des données à un serveur
            console.log('Réservation reçue:', donneesReservation);
        }

        // Réinitialiser le formulaire
        function reinitialiserFormulaire() {
            // Réinitialiser les données
            donneesReservation = {
                nom: '',
                prenom: '',
                telephone: '',
                raison: '',
                creneau: ''
            };
            
            // Réinitialiser les champs
            document.getElementById('nom').value = '';
            document.getElementById('prenom').value = '';
            document.getElementById('telephone').value = '';
            document.getElementById('raison').value = '';
            
            // Réinitialiser la sélection de créneau
            document.querySelectorAll('.creneau-option').forEach(opt => {
                opt.classList.remove('selected');
            });
            document.getElementById('btnConfirmer').disabled = true;
            creneauSelectionne = null;
            
            // Retourner à la page 1
            changerPage(1);
        }

        // Gestion de la sélection des créneaux
        document.addEventListener('DOMContentLoaded', function() {
            document.querySelectorAll('.creneau-option').forEach(option => {
                option.addEventListener('click', function() {
                    // Désélectionner les autres
                    document.querySelectorAll('.creneau-option').forEach(opt => {
                        opt.classList.remove('selected');
                    });
                    
                    // Sélectionner celui-ci
                    this.classList.add('selected');
                    creneauSelectionne = this.getAttribute('data-creneau');
                    
                    // Activer le bouton de confirmation
                    document.getElementById('btnConfirmer').disabled = false;
                });
            });
        });
    </script>
</body>
</html>
