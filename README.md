# leerlingensysteem
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Leerlingensysteem</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        .container {
            max-width: 800px;
            margin: auto;
        }
        .form-section, .list-section {
            margin-bottom: 20px;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        .form-section h2, .list-section h2 {
            margin-top: 0;
        }
        table {
            width: 100%;
            border-collapse: collapse;
        }
        table, th, td {
            border: 1px solid #ccc;
        }
        th, td {
            padding: 10px;
            text-align: left;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Leerlingensysteem</h1>

        <!-- Formulier om een nieuwe leerling toe te voegen -->
        <div class="form-section">
            <h2>Nieuwe Leerling Toevoegen</h2>
            <form id="leerlingForm">
                <label for="naam">Naam:</label><br>
                <input type="text" id="naam" name="naam" required><br><br>

                <label for="leeftijd">Leeftijd:</label><br>
                <input type="number" id="leeftijd" name="leeftijd" required><br><br>

                <label for="klas">Klas:</label><br>
                <input type="text" id="klas" name="klas" required><br><br>

                <label for="status">Status:</label><br>
                <select id="status" name="status" required>
                    <option value="Aanwezig">Aanwezig</option>
                    <option value="Afwezig">Afwezig</option>
                    <option value="Te laat">Te laat</option>
                </select><br><br>

                <label for="email">E-mail:</label><br>
                <input type="email" id="email" name="email" required><br><br>

                <button type="submit">Toevoegen</button>
            </form>
        </div>

        <!-- Lijst van leerlingen -->
        <div class="list-section">
            <h2>Leerlingenlijst</h2>
            <table>
                <thead>
                    <tr>
                        <th>Naam</th>
                        <th>Leeftijd</th>
                        <th>Klas</th>
                        <th>Status</th>
                        <th>E-mail</th>
                        <th>Acties</th>
                    </tr>
                </thead>
                <tbody id="leerlingenLijst">
                    <!-- Leerlingen worden hier dynamisch toegevoegd -->
                </tbody>
            </table>
        </div>
    </div>

    <script>
        // Leerlingen data (in-memory, geen database)
        let leerlingen = [];

        // Formulier ophalen
        const leerlingForm = document.getElementById('leerlingForm');
        const leerlingenLijst = document.getElementById('leerlingenLijst');

        // Leerling toevoegen functie
        leerlingForm.addEventListener('submit', function(event) {
            event.preventDefault(); // Voorkomt het herladen van de pagina

            // Haal de ingevoerde waarden op
            const naam = document.getElementById('naam').value;
            const leeftijd = document.getElementById('leeftijd').value;
            const klas = document.getElementById('klas').value;
            const status = document.getElementById('status').value;
            const email = document.getElementById('email').value;

            // Voeg de nieuwe leerling toe aan de lijst
            leerlingen.push({ naam, leeftijd, klas, status, email });

            // Stuur een mail als de leerling te laat is
            if (status === 'Te laat') {
                sendEmail(email, naam);
            }

            // Reset het formulier
            leerlingForm.reset();

            // Update de tabel
            updateLeerlingenLijst();
        });

        // Leerlingenlijst bijwerken
        function updateLeerlingenLijst() {
            // Leeg de huidige lijst
            leerlingenLijst.innerHTML = '';

            // Voeg elke leerling toe aan de tabel
            leerlingen.forEach((leerling, index) => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${leerling.naam}</td>
                    <td>${leerling.leeftijd}</td>
                    <td>${leerling.klas}</td>
                    <td>${leerling.status}</td>
                    <td>${leerling.email}</td>
                    <td><button onclick="verwijderLeerling(${index})">Verwijderen</button></td>
                `;
                leerlingenLijst.appendChild(row);
            });
        }

        // Leerling verwijderen functie
        function verwijderLeerling(index) {
            leerlingen.splice(index, 1); // Verwijder de leerling uit de array
            updateLeerlingenLijst(); // Update de lijst
        }

        // Functie om een e-mail te "sturen"
        function sendEmail(email, naam) {
            alert(`E-mail verstuurd naar ${email}:\n\nBeste ${naam},\n\nU bent te laat. Zorg ervoor dat u op tijd bent voor uw volgende les.`);
        }
    </script>
</body>
</html>
>
