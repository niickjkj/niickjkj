
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Barra de Pesquisa com Rota</title>
    <style>
        /* CSS para estilizar as barras de pesquisa */
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            background-color: #f4f4f4;
            padding: 20px;
        }

        .search-container {
            position: relative;
            max-width: 600px;
            width: 100%;
            border: 1px solid #ccc;
            border-radius: 4px;
            background: #fff;
            padding: 10px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }

        .search-title {
            font-size: 16px;
            margin-bottom: 10px;
        }

        .search-input {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
        }

        .search-results {
            position: absolute;
            top: 100%;
            left: 0;
            right: 0;
            background: #fff;
            border: 1px solid #ccc;
            border-radius: 4px;
            max-height: 200px;
            overflow-y: auto;
            z-index: 1000;
            display: none;
        }

        .search-results div {
            padding: 10px;
            cursor: pointer;
        }

        .search-results div:hover {
            background-color: #f0f0f0;
        }

        .maps-button {
            display: inline-block;
            margin-top: 10px;
            padding: 10px 20px;
            background-color: #4285F4; /* Cor do Google Maps */
            color: white;
            text-decoration: none;
            border-radius: 5px;
            font-size: 16px;
            transition: background-color 0.3s;
            display: none; /* Inicialmente oculto */
        }
        .maps-button:hover {
            background-color: #357AE8; /* Cor mais escura para hover */
        }
    </style>
</head>
<body>
    <!-- Primeira Barra de Pesquisa -->
    <div class="search-container" id="searchContainer1">
        <div class="search-title">Onde você mora?</div>
        <input type="text" id="searchInput1" class="search-input" placeholder="Digite sua pesquisa...">
        <div id="searchResults1" class="search-results"></div>
    </div>

    <!-- Segunda Barra de Pesquisa -->
    <div class="search-container" id="searchContainer2">
        <div class="search-title">Em qual local deseja ir?</div>
        <input type="text" id="searchInput2" class="search-input" placeholder="Digite sua pesquisa...">
        <div id="searchResults2" class="search-results"></div>
        <a id="mapsButton" class="maps-button" href="#" target="_blank">Traçar Rota no GPS</a>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const searchInput1 = document.getElementById('searchInput1');
            const searchResults1 = document.getElementById('searchResults1');
            const searchContainer1 = document.getElementById('searchContainer1');

            const searchInput2 = document.getElementById('searchInput2');
            const searchResults2 = document.getElementById('searchResults2');
            const mapsButton = document.getElementById('mapsButton');
            const searchContainer2 = document.getElementById('searchContainer2');

            let origin = '';
            let destination = '';

            const items = [
                '15 de Outubro', 'Adélia Giuberti', 'Agrícola', 'Angelo Frechiani',
                'Área Rural de Colatina', 'Barbados', 'Bela Vista', 'Boa Vista',
                'Brisa do Vale', 'Castelo Branco', '25 De Janeiro', 'Aeroporto',
                'Alto Vila Nova', 'Antônio Damiani', 'Ayrton Senna', 'Baunilha',
                'Benjamin Carlos dos Santos', 'Boapaba', 'Carlos Germano Naumann',
                'Centro', 'Cidade Jardim', 'Columbia', 'Esplanada', 'Fioravante Marino',
                'Gordiano Guimarães', 'Honório Fraga', 'Industrial Alves Marques', 'Itapina',
                'João Manoel Meneghelli', 'Lacê', 'Colatina Velha', 'Corrego Do Ouro',
                'Fazenda Vitali', 'Francisco Simonassi', 'Graça Aranha', 'Ifes Itapina',
                'Ipê', 'Jardim Planalto', 'Jose De Anchieta', 'Luiz Iglesias', 'Maria das Graças',
                'Mário Giurizatto', 'Martineli', 'Moacyr Brotas', 'Nossa Senhora Aparecida',
                'Novo Horizonte', 'Operário', 'Parque dos Jacarandás', 'Paul de Graça Aranha',
                'Polo Empresarial', 'Maria Esmênia', 'Marista', 'Moacir Brotas',
                'Morada do Sol', 'Olívio Zanoteli', 'Padre José de Anchieta',
                'Parque Residencial Jardins', 'Perpétuo Socorro', 'Por do Sol', 'Raul Giuberti',
                'Residencial Nobre', 'San Diego', 'Santa Helena', 'Santa Mônica',
                'Santa Terezinha', 'Sagrado Coração de Jesus', 'Santa Cecília',
                'Santa Margarida', 'Santa Teresinha', 'Santo Antônio', 'Santos Dumont',
                'São Braz', 'São Judas Tadeu', 'São Marcos', 'São Miguel', 'São Pedro',
                'São Silvano', 'São Vicente', 'Vicente Suella', 'Vila Amélia',
                'Vila Lenira', 'Vila Nova', 'Vila Real', 'Vista da Serra', 'riviera',
            ];

            function setupSearch(searchInput, searchResults, searchContainer, isOrigin) {
                searchInput.addEventListener('input', function() {
                    const query = searchInput.value.toLowerCase();
                    searchResults.innerHTML = '';

                    if (query) {
                        const filteredItems = items.filter(item => item.toLowerCase().includes(query));

                        filteredItems.forEach(item => {
                            const div = document.createElement('div');
                            div.textContent = item;
                            div.addEventListener('click', () => {
                                searchInput.value = item;
                                searchResults.innerHTML = '';

                                if (isOrigin) {
                                    origin = item;
                                } else {
                                    destination = item;
                                }

                                if (origin && destination) {
                                    // Atualizando o link do botão
                                    mapsButton.href = `https://www.google.com/maps/dir/?api=1&origin=${encodeURIComponent(origin)}&destination=${encodeURIComponent(destination)}`;
                                    mapsButton.style.display = 'inline-block'; // Mostrar botão quando as entradas estiverem válidas
                                }
                            });
                            searchResults.appendChild(div);
                        });

                        searchResults.style.display = 'block';
                    } else {
                        searchResults.style.display = 'none';
                        mapsButton.style.display = 'none'; // Ocultar botão se as entradas são inválidas
                    }
                });

                document.addEventListener('click', function(event) {
                    if (!searchContainer.contains(event.target)) {
                        searchResults.style.display = 'none';
                    }
                });
            }

            // Configura as duas barras de pesquisa
            setupSearch(searchInput1, searchResults1, searchContainer1, true); // Barra de origem
            setupSearch(searchInput2, searchResults2, searchContainer2, false); // Barra de destino
        });
    </script>
</body>
</html>

