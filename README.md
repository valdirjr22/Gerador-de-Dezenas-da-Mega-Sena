<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerador de Dezenas de Loterias</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom font for a cleaner look */
        body {
            font-family: 'Inter', sans-serif;
        }
    </style>
</head>
<body class="min-h-screen bg-gradient-to-br from-green-400 to-blue-500 p-4 flex items-center justify-center">
    <div class="bg-white p-6 md:p-8 rounded-2xl shadow-2xl w-full max-w-4xl border-4 border-yellow-400">
        <h1 class="text-3xl md:text-4xl font-extrabold text-center text-gray-800 mb-4 drop-shadow-md">
            Gerador de Dezenas de Loterias
        </h1>
        <p class="text-center text-gray-600 mb-6 max-w-2xl mx-auto">
            Utilize este aplicativo para gerar dezenas da Mega-Sena ou Quina com base na frequência de números sorteados em concursos anteriores.
        </p>

        <div class="bg-yellow-50 border-l-4 border-yellow-500 text-yellow-700 p-4 rounded-lg mb-6 shadow-inner">
            <p class="font-semibold mb-2">Atenção:</p>
            <p class="text-sm">
                Loterias são jogos de azar e cada sorteio é independente. Este programa utiliza estatísticas de sorteios passados para sugerir dezenas, mas não há garantia de que números que saíram com frequência no passado continuarão a sair no futuro. Jogue com responsabilidade.
            </p>
        </div>

        <div class="mb-6 bg-blue-50 p-5 rounded-xl shadow-inner border border-blue-200">
            <h2 class="text-xl md:text-2xl font-bold text-blue-800 mb-4">
                Como Usar:
            </h2>
            <ol class="list-decimal list-inside text-gray-700 space-y-2">
                <li>
                    <span class="font-semibold">Selecione a Loteria:</span> Escolha entre Mega-Sena e Quina. Os dados históricos serão carregados automaticamente.
                </li>
                <li>
                    <span class="font-semibold">Filtre por Mês e Ano:</span> Use os seletores de Mês e Ano para focar a análise em um período específico.
                </li>
                <li>
                    <span class="font-semibold">Verifique a Análise de Frequência:</span> O sistema calculará e exibirá os números mais frequentes com base nos dados filtrados.
                </li>
                <li>
                    <span class="font-semibold">Escolha as Opções de Geração:</span> Defina quantos jogos deseja gerar e qual estratégia usar (frequência direta ou ponderada).
                </li>
                <li>
                    <span class="font-semibold">Gere Suas Dezenas:</span> Clique no botão "Gerar Dezenas" para obter suas sugestões.
                </li>
            </ol>
        </div>

        <div class="mb-6 bg-gray-50 p-5 rounded-xl shadow-inner">
            <h2 class="text-xl md:text-2xl font-bold text-gray-800 mb-4">
                1. Selecionar Loteria:
            </h2>
            <select
                id="lotterySelect"
                class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-transparent transition duration-200 ease-in-out text-gray-800 bg-white"
            >
                <option value="megaSena">Mega-Sena</option>
                <option value="quina">Quina</option>
            </select>
        </div>

        <div class="mb-6 bg-gray-50 p-5 rounded-xl shadow-inner">
            <label for="pastDraws" class="block text-gray-700 text-lg font-bold mb-3">
                2. Resultados dos Sorteios Anteriores:
            </label>
            <textarea
                id="pastDraws"
                class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-transparent transition duration-200 ease-in-out text-gray-800 h-60 resize-y"
                readonly
            ></textarea>
            <p id="errorMessage" class="text-red-600 text-sm mt-2 font-medium whitespace-pre-wrap"></p>
        </div>

        <div class="mb-6 bg-gray-50 p-5 rounded-xl shadow-inner">
            <h2 class="text-xl md:text-2xl font-bold text-gray-800 mb-4">
                3. Filtrar Sorteios:
            </h2>
            <div class="flex flex-col sm:flex-row gap-4 mb-4">
                <div class="flex-1">
                    <label for="filterMonth" class="block text-gray-700 text-md font-medium mb-2">
                        Mês:
                    </label>
                    <select
                        id="filterMonth"
                        class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-transparent transition duration-200 ease-in-out text-gray-800 bg-white"
                    >
                        <option value="all">Todos os Meses</option>
                        <option value="01">Janeiro</option>
                        <option value="02">Fevereiro</option>
                        <option value="03">Março</option>
                        <option value="04">Abril</option>
                        <option value="05">Maio</option>
                        <option value="06">Junho</option>
                        <option value="07">Julho</option>
                        <option value="08">Agosto</option>
                        <option value="09">Setembro</option>
                        <option value="10">Outubro</option>
                        <option value="11">Novembro</option>
                        <option value="12">Dezembro</option>
                    </select>
                </div>
                <div class="flex-1">
                    <label for="filterYear" class="block text-gray-700 text-md font-medium mb-2">
                        Ano:
                    </label>
                    <select
                        id="filterYear"
                        class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus="ring-green-500 focus:border-transparent transition duration-200 ease-in-out text-gray-800 bg-white"
                    >
                        <option value="all">Todos os Anos</option>
                    </select>
                </div>
            </div>
            <p id="noDrawsMessage" class="text-red-600 text-sm mt-2 font-medium hidden">
                Nenhum sorteio encontrado para os filtros selecionados.
            </p>
        </div>

        <div id="frequencyAnalysisSection" class="mb-6 bg-gray-50 p-5 rounded-xl shadow-inner hidden">
            <h2 class="text-xl md:text-2xl font-bold text-gray-800 mb-4">
                4. Análise de Frequência dos Números (Sorteios Filtrados):
            </h2>
            <div id="mostCommonNumbers" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-5 gap-3">
                </div>
            <p id="frequencyDisclaimer" class="text-gray-600 text-sm mt-3 text-center hidden">
                Exibindo os 20 números mais frequentes.
            </p>
        </div>

        <div class="mb-6 bg-gray-50 p-5 rounded-xl shadow-inner">
            <h2 class="text-xl md:text-2xl font-bold text-gray-800 mb-4">
                5. Opções de Geração:
            </h2>
            <div class="flex flex-col md:flex-row md:items-center gap-4 mb-4">
                <div class="flex-1">
                    <label for="numGames" class="block text-gray-700 text-md font-medium mb-2">
                        Quantos jogos gerar?
                    </label>
                    <input
                        type="number"
                        id="numGames"
                        class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-transparent transition duration-200 ease-in-out text-gray-800"
                        min="1"
                        max="10"
                        value="1"
                    />
                </div>
                <div class="flex-1">
                    <label for="strategy" class="block text-gray-700 text-md font-medium mb-2">
                        Estratégia de Geração:
                    </label>
                    <select
                        id="strategy"
                        class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-transparent transition duration-200 ease-in-out text-gray-800 bg-white"
                    >
                        <option value="frequencia">Mais Frequentes (Direta)</option>
                        <option value="ponderada">Ponderada (Aleatória com Viés)</option>
                    </select>
                </div>
            </div>
            <button
                id="generateGamesButton"
                class="w-full bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-6 rounded-xl shadow-lg transition duration-300 ease-in-out transform hover:scale-105 focus:outline-none focus:ring-4 focus:ring-green-300"
            >
                Gerar Dezenas
            </button>
        </div>

        <div id="generatedGamesSection" class="bg-green-50 p-5 rounded-xl shadow-lg border border-green-200 hidden">
            <h2 class="text-xl md:text-2xl font-bold text-green-800 mb-4">
                6. Seus Jogos Gerados:
            </h2>
            <div id="generatedGamesList" class="grid grid-cols-1 gap-4">
                </div>
        </div>
    </div>

    <script>
        // Data for Mega-Sena and Quina
        const lotteryData = {
            megaSena: {
                numbersPerDraw: 6,
                maxNumber: 60,
                data: `Janeiro/2015

Concurso: 1666 - 03/01/2015 (Sábado)
01 02 23 27 45 51
Concurso: 1667 - 07/01/2015 (Quarta)
07 12 24 44 51 56
Concurso: 1668 - 10/01/2015 (Sábado)
05 40 47 52 55 57
Concurso: 1669 - 14/01/2015 (Quarta)
28 29 31 45 48 49
Concurso: 1670 - 17/01/2015 (Sábado)
01 17 19 30 33 47
Concurso: 1671 - 21/01/2015 (Quarta)
21 27 31 45 55 56
Concurso: 1672 - 24/01/2015 (Sábado)
01 03 05 10 20 42
Concurso: 1673 - 28/01/2015 (Quarta)
05 10 23 24 35 47
Concurso: 1674 - 31/01/2015 (Sábado)
22 30 42 50 58 59
Fevereiro/2015

Concurso: 1675 - 04/02/2015 (Quarta)
03 18 34 35 36 56
Concurso: 1676 - 07/02/2015 (Sábado)
01 02 11 37 48 51
Concurso: 1677 - 11/02/2015 (Quarta)
10 15 28 33 51 52
Concurso: 1678 - 14/02/2015 (Sábado)
09 11 17 19 35 52
Concurso: 1679 - 18/02/2015 (Quarta)
01 02 27 28 46 47
Concurso: 1680 - 21/02/2015 (Sábado)
23 37 38 46 47 51
Concurso: 1681 - 25/02/2015 (Quarta)
01 06 08 11 33 50
Concurso: 1682 - 28/02/2015 (Sábado)
07 13 35 37 39 51
Março/2015

Concurso: 1683 - 04/03/2015 (Quarta)
04 12 32 33 34 48
Concurso: 1684 - 07/03/2015 (Sábado)
09 12 18 31 39 50
Concurso: 1685 - 11/03/2015 (Quarta)
03 18 31 39 46 51
Concurso: 1686 - 14/03/2015 (Sábado)
09 14 37 44 46 55
Concurso: 1687 - 18/03/2015 (Quarta)
15 24 37 46 49 58
Concurso: 1688 - 21/03/2015 (Sábado)
18 23 30 32 42 56
Concurso: 1689 - 25/03/2015 (Quarta)
02 05 13 27 41 53
Concurso: 1690 - 28/03/2015 (Sábado)
21 24 26 35 45 53
Abril/2015

Concurso: 1691 - 01/04/2015 (Quarta)
01 10 24 39 45 58
Concurso: 1692 - 04/04/2015 (Sábado)
11 13 14 27 44 56
Concurso: 1693 - 08/04/2015 (Quarta)
05 15 18 19 21 38
Concurso: 1694 - 11/04/2015 (Sábado)
20 29 34 37 45 57
Concurso: 1695 - 15/04/2015 (Quarta)
24 36 38 43 44 58
Concurso: 1696 - 18/04/2015 (Sábado)
01 12 17 31 37 46
Concurso: 1697 - 22/04/2015 (Quarta)
08 23 30 51 53 58
Concurso: 1698 - 25/04/2015 (Sábado)
11 13 24 28 44 45
Concurso: 1699 - 29/04/2015 (Quarta)
01 06 10 30 33 38
Maio/2015

Concurso: 1700 - 02/05/2015 (Sábado)
15 18 41 48 50 58
Concurso: 1701 - 05/05/2015 (Terça)
03 09 18 32 40 56
Concurso: 1702 - 07/05/2015 (Quinta)
17 19 33 35 39 52
Concurso: 1703 - 09/05/2015 (Sábado)
03 23 26 35 39 49
Concurso: 1704 - 13/05/2015 (Quarta)
01 07 11 32 51 59
Concurso: 1705 - 16/05/2015 (Sábado)
25 27 29 37 50 51
Concurso: 1706 - 20/05/2015 (Quarta)
12 31 34 36 48 56
Concurso: 1707 - 23/05/2015 (Sábado)
07 16 20 27 36 52
Concurso: 1708 - 27/05/2015 (Quarta)
17 29 32 38 45 50
Concurso: 1709 - 30/05/2015 (Sábado)
07 19 30 35 42 47
Junho/2015

Concurso: 1710 - 03/06/2015 (Quarta)
07 21 28 56 58 59
Concurso: 1711 - 06/06/2015 (Sábado)
22 26 38 39 45 50
Concurso: 1712 - 10/06/2015 (Quarta)
02 12 19 29 50 59
Concurso: 1713 - 13/06/2015 (Sábado)
03 10 16 23 27 29
Concurso: 1714 - 17/06/2015 (Quarta)
02 08 24 36 51 52
Concurso: 1715 - 20/06/2015 (Sábado)
16 20 35 40 53 60
Concurso: 1716 - 24/06/2015 (Quarta)
02 25 36 41 42 53
Concurso: 1717 - 27/06/2015 (Sábado)
02 09 16 37 44 58
Julho/2015

Concurso: 1718 - 01/07/2015 (Quarta)
04 30 31 32 47 53
Concurso: 1719 - 04/07/2015 (Sábado)
01 22 31 34 44 54
Concurso: 1720 - 07/07/2015 (Terça)
18 23 31 39 49 57
Concurso: 1721 - 09/07/2015 (Quinta)
04 12 19 33 36 38
Concurso: 1722 - 11/07/2015 (Sábado)
09 23 39 41 49 58
Concurso: 1723 - 15/07/2015 (Quarta)
14 27 30 46 52 60
Concurso: 1724 - 18/07/2015 (Sábado)
27 33 37 39 58 60
Concurso: 1725 - 22/07/2015 (Quarta)
16 18 26 30 31 34
Concurso: 1726 - 25/07/2015 (Sábado)
03 10 42 49 54 57
Concurso: 1727 - 29/07/2015 (Quarta)
04 06 19 20 40 41
Agosto/2015

Concurso: 1728 - 01/08/2015 (Sábado)
03 08 28 39 42 59
Concurso: 1729 - 04/08/2015 (Terça)
07 14 23 43 44 53
Concurso: 1730 - 06/08/2015 (Quinta)
01 10 17 24 42 51
Concurso: 1731 - 08/08/2015 (Sábado)
05 18 27 43 49 59
Concurso: 1732 - 12/08/2015 (Quarta)
25 34 38 45 53 60
Concurso: 1733 - 15/08/2015 (Sábado)
03 05 09 14 41 46
Concurso: 1734 - 19/08/2015 (Quarta)
12 17 25 29 41 52
Concurso: 1735 - 22/08/2015 (Sábado)
06 16 33 34 45 46
Concurso: 1736 - 26/08/2015 (Quarta)
12 13 24 29 32 41
Concurso: 1737 - 29/08/2015 (Sábado)
05 08 42 50 51 59
Setembro/2015

Concurso: 1738 - 02/09/2015 (Quarta)
12 28 37 46 48 53
Concurso: 1739 - 05/09/2015 (Sábado)
09 10 17 32 34 46
Concurso: 1740 - 09/09/2015 (Quarta)
10 15 25 38 47 53
Concurso: 1741 - 12/09/2015 (Sábado)
15 18 20 32 48 49
Concurso: 1742 - 16/09/2015 (Quarta)
11 19 21 23 27 52
Concurso: 1743 - 19/09/2015 (Sábado)
04 10 29 31 34 35
Concurso: 1744 - 23/09/2015 (Quarta)
09 12 14 29 42 56
Concurso: 1745 - 26/09/2015 (Sábado)
09 13 18 47 52 60
Concurso: 1746 - 30/09/2015 (Quarta)
06 18 37 40 41 58
Outubro/2015

Concurso: 1747 - 03/10/2015 (Sábado)
06 33 34 48 53 59
Concurso: 1748 - 07/10/2015 (Quarta)
02 10 13 41 49 53
Concurso: 1749 - 10/10/2015 (Sábado)
03 13 14 29 33 43
Concurso: 1750 - 13/10/2015 (Terça)
15 17 20 31 41 48
Concurso: 1751 - 15/10/2015 (Quinta)
15 22 23 42 51 57
Concurso: 1752 - 17/10/2015 (Sábado)
09 36 37 53 55 60
Concurso: 1753 - 21/10/2015 (Quarta)
08 15 29 35 45 54
Concurso: 1754 - 24/10/2015 (Sábado)
20 27 30 31 40 53
Concurso: 1755 - 28/10/2015 (Quarta)
02 05 08 18 30 48
Concurso: 1756 - 31/10/2015 (Sábado)
06 13 14 28 35 45
Novembro/2015

Concurso: 1757 - 04/11/2015 (Quarta)
13 25 28 37 43 56
Concurso: 1758 - 07/11/2015 (Sábado)
06 11 16 23 36 42
Concurso: 1759 - 10/11/2015 (Terça)
02 14 21 22 51 60
Concurso: 1760 - 13/11/2015 (Sexta)
10 24 25 36 47 48
Concurso: 1761 - 14/11/2015 (Sábado)
09 10 36 50 53 55
Concurso: 1762 - 18/11/2015 (Quarta)
26 32 42 45 55 59
Concurso: 1763 - 21/11/2015 (Sábado)
09 12 15 21 31 36
Concurso: 1764 - 25/11/2015 (Quarta)
06 07 29 39 41 55
Concurso: 1765 - 28/11/2015 (Sábado)
01 06 28 37 56 58
Dezembro/2015

Concurso: 1766 - 02/12/2015 (Quarta)
22 23 41 46 53 60
Concurso: 1767 - 05/12/2015 (Sábado)
16 26 35 39 44 45
Concurso: 1768 - 09/12/2015 (Quarta)
05 07 11 34 35 50
Concurso: 1769 - 12/12/2015 (Sábado)
32 37 44 47 54 60
Concurso: 1770 - 16/12/2015 (Quarta)
11 26 27 30 34 41
Concurso: 1771 - 19/12/2015 (Sábado)
02 20 27 28 32 38
Concurso: 1772 - 22/12/2015 (Terça)
12 19 27 39 41 45
Concurso: 1773 - 24/12/2015 (Quinta)
15 30 39 41 45 59
Concurso: 1774 - 26/12/2015 (Sábado)
01 12 20 30 52 60
Concurso: 1775 - 31/12/2015 (Quinta)
02 18 31 42 51 56
Janeiro/2016

Concurso: 1776 - 02/01/2016 (Sábado)
10 11 14 19 39 48
Concurso: 1777 - 06/01/2016 (Quarta)
04 19 38 44 52 55
Concurso: 1778 - 09/01/2016 (Sábado)
04 10 38 40 45 48
Concurso: 1779 - 12/01/2016 (Terça)
12 25 30 39 56 57
Concurso: 1780 - 14/01/2016 (Quinta)
06 20 22 31 33 34
Concurso: 1781 - 16/01/2016 (Sábado)
01 08 22 49 52 53
Concurso: 1782 - 20/01/2016 (Quarta)
06 18 34 47 52 57
Concurso: 1783 - 23/01/2016 (Sábado)
04 06 16 18 21 38
Concurso: 1784 - 27/01/2016 (Quarta)
04 15 26 30 54 55
Concurso: 1785 - 30/01/2016 (Sábado)
05 11 27 41 42 54
Fevereiro/2016

Concurso: 1786 - 02/02/2016 (Terça)
02 06 08 21 25 34
Concurso: 1787 - 04/02/2016 (Quinta)
01 05 13 25 26 29
Concurso: 1788 - 06/02/2016 (Sábado)
03 13 42 45 56 59
Concurso: 1789 - 10/02/2016 (Quarta)
06 25 43 57 58 59
Concurso: 1790 - 13/02/2016 (Sábado)
11 13 26 27 28 50
Concurso: 1791 - 17/02/2016 (Quarta)
12 18 46 50 53 55
Concurso: 1792 - 20/02/2016 (Sábado)
02 16 17 18 41 47
Concurso: 1793 - 24/02/2016 (Quarta)
13 14 22 54 56 58
Concurso: 1794 - 27/02/2016 (Sábado)
01 05 34 37 40 60
Março/2016

Concurso: 1795 - 02/03/2016 (Quarta)
30 42 47 50 55 58
Concurso: 1796 - 05/03/2016 (Sábado)
22 23 34 48 53 54
Concurso: 1797 - 08/03/2016 (Terça)
02 13 14 35 54 57
Concurso: 1798 - 10/03/2016 (Quinta)
02 09 23 28 45 53
Concurso: 1799 - 12/03/2016 (Sábado)
01 03 04 39 51 53
Concurso: 1800 - 16/03/2016 (Quarta)
06 11 16 19 31 56
Concurso: 1801 - 19/03/2016 (Sábado)
06 19 34 43 45 54
Concurso: 1802 - 23/03/2016 (Quarta)
02 06 13 14 17 44
Concurso: 1803 - 26/03/2016 (Sábado)
04 08 29 38 49 50
Concurso: 1804 - 30/03/2016 (Quarta)
20 21 28 48 50 59
Abril/2016

Concurso: 1805 - 02/04/2016 (Sábado)
17 22 27 31 49 57
Concurso: 1806 - 06/04/2016 (Quarta)
11 20 35 42 55 58
Concurso: 1807 - 09/04/2016 (Sábado)
01 15 16 22 25 43
Concurso: 1808 - 13/04/2016 (Quarta)
02 14 20 25 41 45
Concurso: 1809 - 16/04/2016 (Sábado)
09 12 23 24 46 54
Concurso: 1810 - 20/04/2016 (Quarta)
01 10 25 43 50 56
Concurso: 1811 - 23/04/2016 (Sábado)
05 17 32 35 37 57
Concurso: 1812 - 27/04/2016 (Quarta)
20 23 32 34 37 45
Concurso: 1813 - 30/04/2016 (Sábado)
09 11 13 15 19 51
Maio/2016

Concurso: 1814 - 03/05/2016 (Terça)
09 11 27 46 51 53
Concurso: 1815 - 05/05/2016 (Quinta)
08 11 25 39 41 60
Concurso: 1816 - 07/05/2016 (Sábado)
05 10 12 22 28 46
Concurso: 1817 - 11/05/2016 (Quarta)
01 04 26 39 47 55
Concurso: 1818 - 14/05/2016 (Sábado)
02 06 10 15 53 56
Concurso: 1819 - 18/05/2016 (Quarta)
17 18 26 30 33 37
Concurso: 1820 - 21/05/2016 (Sábado)
03 19 23 27 40 45
Concurso: 1821 - 25/05/2016 (Quarta)
19 22 31 36 52 53
Concurso: 1822 - 28/05/2016 (Sábado)
01 22 26 43 50 53
Junho/2016

Concurso: 1823 - 01/06/2016 (Quarta)
04 09 21 34 54 59
Concurso: 1824 - 04/06/2016 (Sábado)
05 06 12 19 30 60
Concurso: 1825 - 07/06/2016 (Terça)
10 11 21 50 51 54
Concurso: 1826 - 09/06/2016 (Quinta)
17 19 32 43 48 51
Concurso: 1827 - 11/06/2016 (Sábado)
26 33 42 43 53 54
Concurso: 1828 - 15/06/2016 (Quarta)
06 11 32 40 48 59
Concurso: 1829 - 18/06/2016 (Sábado)
07 13 24 30 32 53
Concurso: 1830 - 22/06/2016 (Quarta)
03 07 29 37 54 60
Concurso: 1831 - 25/06/2016 (Sábado)
15 27 28 32 48 55
Concurso: 1832 - 29/06/2016 (Quarta)
14 34 46 47 56 57
Julho/2016

Concurso: 1833 - 02/07/2016 (Sábado)
02 03 07 16 27 42
Concurso: 1834 - 06/07/2016 (Quarta)
02 17 22 24 48 51
Concurso: 1835 - 09/07/2016 (Sábado)
08 28 36 47 50 58
Concurso: 1836 - 12/07/2016 (Terça)
08 15 18 45 59 60
Concurso: 1837 - 14/07/2016 (Quinta)
41 44 48 50 54 57
Concurso: 1838 - 16/07/2016 (Sábado)
05 08 24 30 57 59
Concurso: 1839 - 20/07/2016 (Quarta)
07 22 28 32 56 58
Concurso: 1840 - 23/07/2016 (Sábado)
15 17 33 41 47 48
Concurso: 1841 - 27/07/2016 (Quarta)
03 06 13 38 49 51
Concurso: 1842 - 30/07/2016 (Sábado)
16 18 22 24 34 43
Agosto/2016

Concurso: 1843 - 03/08/2016 (Quarta)
08 26 28 33 41 54
Concurso: 1844 - 06/08/2016 (Sábado)
03 04 37 39 48 50
Concurso: 1845 - 09/08/2016 (Terça)
03 09 13 29 30 51
Concurso: 1846 - 11/08/2016 (Quinta)
06 24 26 30 34 49
Concurso: 1847 - 13/08/2016 (Sábado)
01 06 45 49 50 57
Concurso: 1848 - 17/08/2016 (Quarta)
09 40 41 50 55 58
Concurso: 1849 - 20/08/2016 (Sábado)
03 05 11 27 35 44
Concurso: 1850 - 24/08/2016 (Quarta)
23 24 32 38 40 41
Concurso: 1851 - 27/08/2016 (Sábado)
08 18 21 22 35 37
Concurso: 1852 - 31/08/2016 (Quarta)
13 17 29 45 49 50
Setembro/2016

Concurso: 1853 - 03/09/2016 (Sábado)
01 02 34 39 41 45
Concurso: 1854 - 08/09/2016 (Quinta)
25 30 31 34 43 59
Concurso: 1855 - 10/09/2016 (Sábado)
06 10 15 24 38 39
Concurso: 1856 - 14/09/2016 (Quarta)
02 09 14 22 32 37
Concurso: 1857 - 17/09/2016 (Sábado)
13 23 25 35 52 53
Concurso: 1858 - 20/09/2016 (Terça)
22 28 30 33 55 59
Concurso: 1859 - 22/09/2016 (Quinta)
01 13 14 21 26 51
Concurso: 1860 - 24/09/2016 (Sábado)
10 30 36 40 44 60
Concurso: 1861 - 28/09/2016 (Quarta)
02 04 09 35 45 60
Outubro/2016

Concurso: 1862 - 01/10/2016 (Sábado)
02 08 35 42 49 56
Concurso: 1863 - 05/10/2016 (Quarta)
16 23 45 56 58 59
Concurso: 1864 - 08/10/2016 (Sábado)
04 05 14 37 40 60
Concurso: 1865 - 13/10/2016 (Quinta)
01 05 31 32 37 42
Concurso: 1866 - 15/10/2016 (Sábado)
14 17 36 38 44 60
Concurso: 1867 - 18/10/2016 (Terça)
03 10 17 21 22 43
Concurso: 1868 - 20/10/2016 (Quinta)
01 05 23 25 28 31
Concurso: 1869 - 22/10/2016 (Sábado)
11 23 24 26 40 52
Concurso: 1870 - 26/10/2016 (Quarta)
18 20 30 32 33 40
Concurso: 1871 - 29/10/2016 (Sábado)
03 11 17 33 52 58
Novembro/2016

Concurso: 1872 - 03/11/2016 (Quinta)
11 13 25 39 46 56
Concurso: 1873 - 05/11/2016 (Sábado)
05 25 28 41 53 54
Concurso: 1874 - 08/11/2016 (Terça)
10 28 37 43 44 47
Concurso: 1875 - 10/11/2016 (Quinta)
01 45 47 52 53 55
Concurso: 1876 - 12/11/2016 (Sábado)
07 18 39 41 44 51
Concurso: 1877 - 16/11/2016 (Quarta)
13 16 23 24 32 35
Concurso: 1878 - 19/11/2016 (Sábado)
12 16 26 40 56 57
Concurso: 1879 - 23/11/2016 (Quarta)
05 10 20 57 58 59
Concurso: 1880 - 26/11/2016 (Sábado)
05 16 19 37 51 56
Concurso: 1881 - 30/11/2016 (Quarta)
03 10 30 44 53 56
Dezembro/2016

Concurso: 1882 - 03/12/2016 (Sábado)
09 10 19 35 37 41
Concurso: 1883 - 07/12/2016 (Quarta)
16 27 28 47 59 60
Concurso: 1884 - 10/12/2016 (Sábado)
01 04 23 32 38 59
Concurso: 1885 - 14/12/2016 (Quarta)
07 14 18 23 32 59
Concurso: 1886 - 17/12/2016 (Sábado)
03 07 15 40 45 54
Concurso: 1887 - 20/12/2016 (Terça)
11 23 34 41 46 56
Concurso: 1888 - 22/12/2016 (Quinta)
01 10 17 18 45 48
Concurso: 1889 - 24/12/2016 (Sábado)
16 23 25 28 30 44
Concurso: 1890 - 31/12/2016 (Sábado)
05 11 22 24 51 53
Janeiro/2017

Concurso: 1891 - 04/01/2017 (Quarta)
01 03 19 23 47 58
Concurso: 1892 - 07/01/2017 (Sábado)
06 17 22 30 37 50
Concurso: 1893 - 11/01/2017 (Quarta)
16 17 19 28 45 58
Concurso: 1894 - 14/01/2017 (Sábado)
21 31 35 53 54 57
Concurso: 1895 - 18/01/2017 (Quarta)
02 03 05 10 15 34
Concurso: 1896 - 21/01/2017 (Sábado)
03 06 14 15 21 25
Concurso: 1897 - 25/01/2017 (Quarta)
09 22 25 47 52 58
Concurso: 1898 - 28/01/2017 (Sábado)
12 34 45 53 55 58
Fevereiro/2017

Concurso: 1899 - 01/02/2017 (Quarta)
20 23 35 36 44 48
Concurso: 1900 - 04/02/2017 (Sábado)
08 11 27 28 43 46
Concurso: 1901 - 08/02/2017 (Quarta)
11 12 26 30 37 53
Concurso: 1902 - 11/02/2017 (Sábado)
02 07 09 18 21 25
Concurso: 1903 - 15/02/2017 (Quarta)
09 10 15 28 43 45
Concurso: 1904 - 18/02/2017 (Sábado)
12 15 18 21 51 56
Concurso: 1905 - 21/02/2017 (Terça)
29 35 43 54 56 57
Concurso: 1906 - 23/02/2017 (Quinta)
06 27 33 39 40 60
Concurso: 1907 - 25/02/2017 (Sábado)
03 25 35 38 44 48
Março/2017

Concurso: 1908 - 01/03/2017 (Quarta)
04 10 13 23 27 57
Concurso: 1909 - 04/03/2017 (Sábado)
11 40 43 45 47 57
Concurso: 1910 - 08/03/2017 (Quarta)
06 09 15 22 39 48
Concurso: 1911 - 11/03/2017 (Sábado)
16 18 23 30 32 33
Concurso: 1912 - 15/03/2017 (Quarta)
10 13 33 35 36 42
Concurso: 1913 - 18/03/2017 (Sábado)
04 14 17 43 52 56
Concurso: 1914 - 22/03/2017 (Quarta)
16 29 33 39 42 44
Concurso: 1915 - 25/03/2017 (Sábado)
02 20 21 33 48 57
Concurso: 1916 - 29/03/2017 (Quarta)
03 09 18 23 50 52
Abril/2017

Concurso: 1917 - 01/04/2017 (Sábado)
04 21 25 33 36 46
Concurso: 1918 - 05/04/2017 (Quarta)
16 17 20 22 38 50
Concurso: 1919 - 08/04/2017 (Sábado)
11 28 37 45 54 60
Concurso: 1920 - 12/04/2017 (Quarta)
25 31 33 39 43 45
Concurso: 1921 - 15/04/2017 (Sábado)
10 15 16 19 28 35
Concurso: 1922 - 19/04/2017 (Quarta)
20 22 36 38 41 43
Concurso: 1923 - 22/04/2017 (Sábado)
09 34 42 45 46 59
Concurso: 1924 - 26/04/2017 (Quarta)
12 16 30 52 53 58
Concurso: 1925 - 29/04/2017 (Sábado)
01 17 38 43 45 47
Maio/2017

Concurso: 1926 - 03/05/2017 (Quarta)
02 03 14 20 30 46
Concurso: 1927 - 06/05/2017 (Sábado)
05 11 13 19 30 31
Concurso: 1928 - 09/05/2017 (Terça)
10 26 28 51 53 59
Concurso: 1929 - 11/05/2017 (Quinta)
03 10 23 36 43 52
Concurso: 1930 - 13/05/2017 (Sábado)
04 17 18 37 52 56
Concurso: 1931 - 17/05/2017 (Quarta)
02 08 09 15 22 34
Concurso: 1932 - 20/05/2017 (Sábado)
10 16 21 29 44 55
Concurso: 1933 - 24/05/2017 (Quarta)
02 14 15 19 35 59
Concurso: 1934 - 27/05/2017 (Sábado)
08 23 35 39 56 59
Concurso: 1935 - 31/05/2017 (Quarta)
01 03 10 17 23 24
Junho/2017

Concurso: 1936 - 03/06/2017 (Sábado)
03 12 24 40 51 52
Concurso: 1937 - 07/06/2017 (Quarta)
06 10 24 29 43 55
Concurso: 1938 - 10/06/2017 (Sábado)
10 16 32 40 42 54
Concurso: 1939 - 14/06/2017 (Quarta)
07 24 29 39 45 52
Concurso: 1940 - 17/06/2017 (Sábado)
09 13 16 17 36 47
Concurso: 1941 - 21/06/2017 (Quarta)
01 09 24 38 48 49
Concurso: 1942 - 24/06/2017 (Sábado)
06 18 20 24 43 48
Concurso: 1943 - 28/06/2017 (Quarta)
09 11 12 30 39 43
Julho/2017

Concurso: 1944 - 01/07/2017 (Sábado)
08 09 39 47 57 59
Concurso: 1945 - 04/07/2017 (Terça)
04 08 10 21 45 54
Concurso: 1946 - 06/07/2017 (Quinta)
04 06 39 44 52 60
Concurso: 1947 - 08/07/2017 (Sábado)
08 33 40 52 55 59
Concurso: 1948 - 12/07/2017 (Quarta)
12 20 24 28 33 57
Concurso: 1949 - 15/07/2017 (Sábado)
01 06 14 22 30 56
Concurso: 1950 - 19/07/2017 (Quarta)
10 21 32 34 48 57
Concurso: 1951 - 22/07/2017 (Sábado)
14 16 19 21 33 55
Concurso: 1952 - 26/07/2017 (Quarta)
09 21 36 38 52 53
Concurso: 1953 - 29/07/2017 (Sábado)
09 26 29 42 43 45
Agosto/2017

Concurso: 1954 - 02/08/2017 (Quarta)
09 25 33 35 40 49
Concurso: 1955 - 05/08/2017 (Sábado)
15 27 33 36 41 45
Concurso: 1956 - 08/08/2017 (Terça)
05 08 20 28 40 45
Concurso: 1957 - 10/08/2017 (Quinta)
02 14 22 23 29 47
Concurso: 1958 - 12/08/2017 (Sábado)
15 20 22 24 34 55
Concurso: 1959 - 16/08/2017 (Quarta)
08 21 27 35 43 56
Concurso: 1960 - 19/08/2017 (Sábado)
01 18 25 37 39 43
Concurso: 1961 - 23/08/2017 (Quarta)
17 25 26 30 32 50
Concurso: 1962 - 26/08/2017 (Sábado)
05 06 15 25 39 53
Concurso: 1963 - 30/08/2017 (Quarta)
04 05 07 34 42 60
Setembro/2017

Concurso: 1964 - 02/09/2017 (Sábado)
02 27 32 36 48 50
Concurso: 1965 - 06/09/2017 (Quarta)
26 28 35 38 48 55
Concurso: 1966 - 09/09/2017 (Sábado)
10 13 19 32 40 60
Concurso: 1967 - 13/09/2017 (Quarta)
13 30 32 39 46 54
Concurso: 1968 - 16/09/2017 (Sábado)
35 36 39 44 48 52
Concurso: 1969 - 19/09/2017 (Terça)
08 20 30 32 48 59
Concurso: 1970 - 21/09/2017 (Quinta)
05 10 18 24 39 52
Concurso: 1971 - 23/09/2017 (Sábado)
04 10 41 44 52 54
Concurso: 1972 - 27/09/2017 (Quarta)
09 16 20 54 57 59
Concurso: 1973 - 30/09/2017 (Sábado)
01 12 16 17 52 60
Outubro/2017

Concurso: 1974 - 04/10/2017 (Quarta)
04 19 27 38 54 57
Concurso: 1975 - 07/10/2017 (Sábado)
08 11 24 26 47 57
Concurso: 1976 - 11/10/2017 (Quarta)
12 14 19 25 52 59
Concurso: 1977 - 14/10/2017 (Sábado)
03 16 28 32 34 37
Concurso: 1978 - 17/10/2017 (Terça)
02 06 22 44 55 57
Concurso: 1979 - 19/10/2017 (Quinta)
22 23 29 32 38 45
Concurso: 1980 - 21/10/2017 (Sábado)
12 16 17 18 34 37
Concurso: 1981 - 25/10/2017 (Quarta)
06 15 19 37 39 53
Concurso: 1982 - 28/10/2017 (Sábado)
04 14 20 24 46 50
Novembro/2017

Concurso: 1983 - 01/11/2017 (Quarta)
05 06 10 14 22 36
Concurso: 1984 - 04/11/2017 (Sábado)
21 29 32 35 43 50
Concurso: 1985 - 07/11/2017 (Terça)
09 28 29 34 36 55
Concurso: 1986 - 09/11/2017 (Quinta)
23 36 43 44 55 56
Concurso: 1987 - 11/11/2017 (Sábado)
10 14 31 34 45 58
Concurso: 1988 - 16/11/2017 (Quinta)
05 10 39 42 46 54
Concurso: 1989 - 18/11/2017 (Sábado)
15 22 30 32 40 58
Concurso: 1990 - 22/11/2017 (Quarta)
11 24 26 34 37 59
Concurso: 1991 - 25/11/2017 (Sábado)
19 20 28 34 36 44
Concurso: 1992 - 29/11/2017 (Quarta)
05 11 13 21 53 54
Dezembro/2017

Concurso: 1993 - 02/12/2017 (Sábado)
06 17 33 48 50 57
Concurso: 1994 - 06/12/2017 (Quarta)
02 05 12 32 40 44
Concurso: 1995 - 09/12/2017 (Sábado)
14 26 29 35 37 39
Concurso: 1996 - 13/12/2017 (Quarta)
07 20 21 24 40 56
Concurso: 1997 - 16/12/2017 (Sábado)
01 07 14 31 35 46
Concurso: 1998 - 19/12/2017 (Terça)
08 21 24 25 52 57
Concurso: 1999 - 21/12/2017 (Quinta)
15 37 38 42 49 50
Concurso: 2000 - 31/12/2017 (Domingo)
03 06 10 17 34 37
Janeiro/2018

Concurso: 2001 - 03/01/2018 (Quarta)
20 22 36 42 52 60
Concurso: 2002 - 06/01/2018 (Sábado)
04 28 30 38 46 59
Concurso: 2003 - 10/01/2018 (Quarta)
28 34 40 43 50 51
Concurso: 2004 - 13/01/2018 (Sábado)
01 05 14 23 35 45
Concurso: 2005 - 17/01/2018 (Quarta)
11 19 22 33 34 53
Concurso: 2006 - 20/01/2018 (Sábado)
01 09 14 20 25 54
Concurso: 2007 - 24/01/2018 (Quarta)
04 14 39 41 54 58
Concurso: 2008 - 27/01/2018 (Sábado)
22 27 33 42 58 59
Concurso: 2009 - 31/01/2018 (Quarta)
01 37 44 46 48 50
Fevereiro/2018

Concurso: 2010 - 03/02/2018 (Sábado)
08 10 18 34 39 56
Concurso: 2011 - 06/02/2018 (Terça)
02 28 32 35 54 58
Concurso: 2012 - 08/02/2018 (Quinta)
08 11 27 35 36 51
Concurso: 2013 - 10/02/2018 (Sábado)
06 23 30 36 53 56
Concurso: 2014 - 14/02/2018 (Quarta)
16 32 40 46 53 56
Concurso: 2015 - 17/02/2018 (Sábado)
17 18 27 32 39 58
Concurso: 2016 - 21/02/2018 (Quarta)
04 11 17 18 21 48
Concurso: 2017 - 24/02/2018 (Sábado)
02 10 11 24 38 56
Concurso: 2018 - 28/02/2018 (Quarta)
11 22 25 27 55 59
Março/2018

Concurso: 2019 - 03/03/2018 (Sábado)
23 41 46 52 54 59
Concurso: 2020 - 07/03/2018 (Quarta)
02 36 46 48 57 60
Concurso: 2021 - 10/03/2018 (Sábado)
07 14 32 37 40 60
Concurso: 2022 - 14/03/2018 (Quarta)
04 28 45 48 52 60
Concurso: 2023 - 17/03/2018 (Sábado)
01 06 07 08 23 56
Concurso: 2024 - 21/03/2018 (Quarta)
16 20 23 29 35 47
Concurso: 2025 - 24/03/2018 (Sábado)
04 24 46 52 55 56
Concurso: 2026 - 28/03/2018 (Quarta)
10 23 31 33 51 52
Concurso: 2027 - 31/03/2018 (Sábado)
11 15 29 37 39 44
Abril/2018

Concurso: 2028 - 04/04/2018 (Quarta)
07 11 24 36 42 58
Concurso: 2029 - 07/04/2018 (Sábado)
06 15 18 33 37 40
Concurso: 2030 - 11/04/2018 (Quarta)
04 27 38 40 58 59
Concurso: 2031 - 14/04/2018 (Sábado)
18 23 37 39 50 55
Concurso: 2032 - 17/04/2018 (Terça)
06 14 19 20 39 53
Concurso: 2033 - 20/04/2018 (Sexta)
10 18 33 38 40 43
Concurso: 2034 - 25/04/2018 (Quarta)
06 07 13 17 22 56
Concurso: 2035 - 28/04/2018 (Sábado)
30 35 36 38 49 52
Maio/2018

Concurso: 2036 - 02/05/2018 (Quarta)
07 08 19 23 27 58
Concurso: 2037 - 05/05/2018 (Sábado)
14 16 23 30 45 60
Concurso: 2038 - 08/05/2018 (Terça)
06 25 26 35 38 40
Concurso: 2039 - 10/05/2018 (Quinta)
06 12 22 28 31 44
Concurso: 2040 - 12/05/2018 (Sábado)
06 09 41 54 56 58
Concurso: 2041 - 16/05/2018 (Quarta)
10 12 22 25 42 54
Concurso: 2042 - 19/05/2018 (Sábado)
14 22 29 32 33 35
Concurso: 2043 - 23/05/2018 (Quarta)
21 38 53 56 57 58
Concurso: 2044 - 26/05/2018 (Sábado)
07 14 47 54 56 60
Concurso: 2045 - 30/05/2018 (Quarta)
15 25 27 45 46 50
Junho/2018

Concurso: 2046 - 02/06/2018 (Sábado)
03 06 11 27 28 46
Concurso: 2047 - 06/06/2018 (Quarta)
01 18 19 29 44 54
Concurso: 2048 - 09/06/2018 (Sábado)
10 19 26 35 38 39
Concurso: 2049 - 13/06/2018 (Quarta)
10 19 27 31 51 53
Concurso: 2050 - 16/06/2018 (Sábado)
08 31 32 33 38 50
Concurso: 2051 - 20/06/2018 (Quarta)
01 05 06 37 44 53
Concurso: 2052 - 23/06/2018 (Sábado)
50 51 56 57 58 59
Concurso: 2053 - 27/06/2018 (Quarta)
12 22 27 37 40 55
Concurso: 2054 - 30/06/2018 (Sábado)
04 07 12 22 26 39
Julho/2018

Concurso: 2055 - 03/07/2018 (Terça)
06 25 35 43 46 53
Concurso: 2056 - 05/07/2018 (Quinta)
18 22 29 34 36 47
Concurso: 2057 - 07/07/2018 (Sábado)
10 13 20 37 38 54
Concurso: 2058 - 11/07/2018 (Quarta)
04 19 23 29 56 59
Concurso: 2059 - 14/07/2018 (Sábado)
04 05 36 40 44 56
Concurso: 2060 - 18/07/2018 (Quarta)
08 09 11 25 39 41
Concurso: 2061 - 21/07/2018 (Sábado)
33 36 40 44 45 54
Concurso: 2062 - 25/07/2018 (Quarta)
08 10 15 23 25 34
Concurso: 2063 - 28/07/2018 (Sábado)
06 10 19 24 25 29
Agosto/2018

Concurso: 2064 - 01/08/2018 (Quarta)
10 14 36 53 55 60
Concurso: 2065 - 04/08/2018 (Sábado)
04 11 20 30 37 43
Concurso: 2066 - 08/08/2018 (Quarta)
06 25 27 35 45 55
Concurso: 2067 - 11/08/2018 (Sábado)
02 11 13 26 32 59
Concurso: 2068 - 14/08/2018 (Terça)
03 05 09 40 42 47
Concurso: 2069 - 16/08/2018 (Quinta)
03 17 34 35 40 48
Concurso: 2070 - 18/08/2018 (Sábado)
05 26 27 34 42 48
Concurso: 2071 - 22/08/2018 (Quarta)
24 33 34 35 46 60
Concurso: 2072 - 25/08/2018 (Sábado)
10 12 13 20 22 54
Concurso: 2073 - 29/08/2018 (Quarta)
12 15 18 30 52 55
Setembro/2018

Concurso: 2074 - 01/09/2018 (Sábado)
08 18 23 37 42 58
Concurso: 2075 - 05/09/2018 (Quarta)
07 09 23 33 57 59
Concurso: 2076 - 08/09/2018 (Sábado)
05 06 12 15 22 43
Concurso: 2077 - 12/09/2018 (Quarta)
13 16 26 35 37 39
Concurso: 2078 - 15/09/2018 (Sábado)
02 11 15 30 36 39
Concurso: 2079 - 18/09/2018 (Terça)
01 02 14 37 55 58
Concurso: 2080 - 20/09/2018 (Quinta)
10 22 40 46 55 58
Concurso: 2081 - 22/09/2018 (Sábado)
13 18 35 40 41 42
Concurso: 2082 - 26/09/2018 (Quarta)
06 25 33 42 48 49
Concurso: 2083 - 29/09/2018 (Sábado)
01 18 19 33 56 60
Outubro/2018

Concurso: 2084 - 03/10/2018 (Quarta)
07 20 26 37 38 39
Concurso: 2085 - 06/10/2018 (Sábado)
12 21 25 37 38 49
Concurso: 2086 - 10/10/2018 (Quarta)
04 35 43 46 47 53
Concurso: 2087 - 13/10/2018 (Sábado)
02 18 19 23 34 53
Concurso: 2088 - 17/10/2018 (Quarta)
03 14 24 27 38 56
Concurso: 2089 - 20/10/2018 (Sábado)
05 10 32 38 48 49
Concurso: 2090 - 23/10/2018 (Terça)
08 11 18 37 40 51
Concurso: 2091 - 25/10/2018 (Quinta)
10 11 12 37 38 59
Concurso: 2092 - 27/10/2018 (Sábado)
11 13 15 17 22 27
Concurso: 2093 - 31/10/2018 (Quarta)
08 14 27 34 52 54
Novembro/2018

Concurso: 2094 - 03/11/2018 (Sábado)
04 16 19 31 33 44
Concurso: 2095 - 07/11/2018 (Quarta)
16 29 35 43 49 56
Concurso: 2096 - 10/11/2018 (Sábado)
06 11 13 19 24 51
Concurso: 2097 - 14/11/2018 (Quarta)
09 24 28 45 49 51
Concurso: 2098 - 17/11/2018 (Sábado)
02 08 18 27 38 60
Concurso: 2099 - 21/11/2018 (Quarta)
05 15 20 27 30 58
Concurso: 2100 - 24/11/2018 (Sábado)
01 10 11 13 35 49
Concurso: 2101 - 28/11/2018 (Quarta)
02 08 18 37 56 58
Dezembro/2018

Concurso: 2102 - 01/12/2018 (Sábado)
04 06 17 34 51 57
Concurso: 2103 - 04/12/2018 (Terça)
03 13 40 44 46 50
Concurso: 2104 - 06/12/2018 (Quinta)
02 10 12 27 45 56
Concurso: 2105 - 08/12/2018 (Sábado)
11 13 16 24 31 46
Concurso: 2106 - 12/12/2018 (Quarta)
03 27 36 39 40 43
Concurso: 2107 - 15/12/2018 (Sábado)
08 38 44 50 56 60
Concurso: 2108 - 18/12/2018 (Terça)
19 22 29 41 44 59
Concurso: 2109 - 20/12/2018 (Quinta)
04 12 16 34 44 49
Concurso: 2110 - 31/12/2018 (Segunda)
05 10 12 18 25 33
Janeiro/2019

Concurso: 2111 - 02/01/2019 (Quarta)
01 41 44 46 54 58
Concurso: 2112 - 05/01/2019 (Sábado)
17 39 43 46 52 53
Concurso: 2113 - 09/01/2019 (Quarta)
11 14 21 25 46 50
Concurso: 2114 - 12/01/2019 (Sábado)
17 25 30 35 42 57
Concurso: 2115 - 15/01/2019 (Terça)
04 19 27 35 40 44
Concurso: 2116 - 17/01/2019 (Quinta)
01 09 19 21 34 54
Concurso: 2117 - 19/01/2019 (Sábado)
04 28 29 30 43 52
Concurso: 2118 - 23/01/2019 (Quarta)
11 12 20 40 41 46
Concurso: 2119 - 26/01/2019 (Sábado)
19 21 26 31 42 49
Concurso: 2120 - 30/01/2019 (Quarta)
13 20 24 25 38 41
Fevereiro/2019

Concurso: 2121 - 02/02/2019 (Sábado)
08 10 17 29 37 40
Concurso: 2122 - 06/02/2019 (Quarta)
03 11 15 21 27 49
Concurso: 2123 - 09/02/2019 (Sábado)
14 15 47 50 56 59
Concurso: 2124 - 13/02/2019 (Quarta)
02 11 20 31 43 47
Concurso: 2125 - 16/02/2019 (Sábado)
01 31 44 46 53 58
Concurso: 2126 - 20/02/2019 (Quarta)
07 12 24 27 39 58
Concurso: 2127 - 23/02/2019 (Sábado)
01 07 28 30 44 46
Concurso: 2128 - 26/02/2019 (Terça)
11 16 24 46 54 55
Concurso: 2129 - 28/02/2019 (Quinta)
06 12 31 32 46 60
Março/2019

Concurso: 2130 - 02/03/2019 (Sábado)
13 16 36 53 54 55
Concurso: 2131 - 06/03/2019 (Quarta)
02 03 06 18 20 28
Concurso: 2132 - 09/03/2019 (Sábado)
05 18 30 35 39 60
Concurso: 2133 - 13/03/2019 (Quarta)
19 20 26 51 52 57
Concurso: 2134 - 16/03/2019 (Sábado)
06 21 34 46 54 59
Concurso: 2135 - 20/03/2019 (Quarta)
09 23 28 40 48 59
Concurso: 2136 - 23/03/2019 (Sábado)
01 08 11 22 30 35
Concurso: 2137 - 27/03/2019 (Quarta)
01 02 11 12 34 49
Concurso: 2138 - 30/03/2019 (Sábado)
04 13 14 21 30 34
Abril/2019

Concurso: 2139 - 03/04/2019 (Quarta)
14 23 29 41 57 58
Concurso: 2140 - 06/04/2019 (Sábado)
17 20 26 36 42 54
Concurso: 2141 - 10/04/2019 (Quarta)
10 11 17 19 37 41
Concurso: 2142 - 13/04/2019 (Sábado)
07 40 44 50 52 57
Concurso: 2143 - 17/04/2019 (Quarta)
02 12 35 51 57 58
Concurso: 2144 - 20/04/2019 (Sábado)
07 16 21 33 55 60
Concurso: 2145 - 24/04/2019 (Quarta)
06 08 28 51 53 59
Concurso: 2146 - 27/04/2019 (Sábado)
16 18 31 39 42 44
Maio/2019

Concurso: 2147 - 02/05/2019 (Quinta)
17 19 37 41 42 49
Concurso: 2148 - 04/05/2019 (Sábado)
08 15 32 33 58 59
Concurso: 2149 - 08/05/2019 (Quarta)
21 23 37 44 46 48
Concurso: 2150 - 11/05/2019 (Sábado)
23 24 26 38 42 49
Concurso: 2151 - 15/05/2019 (Quarta)
02 14 18 29 36 38
Concurso: 2152 - 18/05/2019 (Sábado)
26 29 36 49 50 59
Concurso: 2153 - 22/05/2019 (Quarta)
08 13 28 31 32 33
Concurso: 2154 - 25/05/2019 (Sábado)
07 25 41 47 50 53
Concurso: 2155 - 29/05/2019 (Quarta)
02 06 27 37 44 47
Junho/2019

Concurso: 2156 - 01/06/2019 (Sábado)
01 06 23 26 39 49
Concurso: 2157 - 05/06/2019 (Quarta)
31 33 34 35 39 48
Concurso: 2158 - 08/06/2019 (Sábado)
09 27 35 45 46 59
Concurso: 2159 - 12/06/2019 (Quarta)
14 26 35 38 45 53
Concurso: 2160 - 15/06/2019 (Sábado)
01 19 46 47 49 53
Concurso: 2161 - 19/06/2019 (Quarta)
08 09 10 24 42 44
Concurso: 2162 - 22/06/2019 (Sábado)
11 16 22 30 34 42
Concurso: 2163 - 26/06/2019 (Quarta)
08 18 20 24 36 45
Concurso: 2164 - 29/06/2019 (Sábado)
16 17 25 47 48 58
Julho/2019

Concurso: 2165 - 03/07/2019 (Quarta)
05 37 43 49 54 56
Concurso: 2166 - 06/07/2019 (Sábado)
03 19 34 44 56 58
Concurso: 2167 - 09/07/2019 (Terça)
27 37 38 43 45 54
Concurso: 2168 - 11/07/2019 (Quinta)
01 04 25 27 29 37
Concurso: 2169 - 13/07/2019 (Sábado)
07 34 45 51 54 59
Concurso: 2170 - 17/07/2019 (Quarta)
10 21 24 36 38 51
Concurso: 2171 - 20/07/2019 (Sábado)
12 13 19 36 44 55
Concurso: 2172 - 24/07/2019 (Quarta)
09 24 28 37 43 57
Concurso: 2173 - 27/07/2019 (Sábado)
02 09 42 44 48 50
Concurso: 2174 - 31/07/2019 (Quarta)
10 15 34 36 56 60
Agosto/2019

Concurso: 2175 - 03/08/2019 (Sábado)
07 25 32 43 53 55
Concurso: 2176 - 06/08/2019 (Terça)
08 23 25 39 43 44
Concurso: 2177 - 08/08/2019 (Quinta)
09 11 14 31 48 51
Concurso: 2178 - 10/08/2019 (Sábado)
02 16 21 42 50 56
Concurso: 2179 - 14/08/2019 (Quarta)
02 13 24 35 50 54
Concurso: 2180 - 17/08/2019 (Sábado)
10 12 16 21 28 38
Concurso: 2181 - 21/08/2019 (Quarta)
01 08 19 33 36 48
Concurso: 2182 - 24/08/2019 (Sábado)
19 22 39 46 47 59
Concurso: 2183 - 28/08/2019 (Quarta)
13 26 30 34 43 51
Concurso: 2184 - 31/08/2019 (Sábado)
15 36 45 51 52 59
Setembro/2019

Concurso: 2185 - 04/09/2019 (Quarta)
09 18 19 22 42 47
Concurso: 2186 - 09/09/2019 (Segunda)
12 18 19 27 41 46
Concurso: 2187 - 11/09/2019 (Quarta)
10 11 16 21 46 50
Concurso: 2188 - 14/09/2019 (Sábado)
02 17 21 28 51 60
Concurso: 2189 - 18/09/2019 (Quarta)
04 11 16 22 29 33
Concurso: 2190 - 21/09/2019 (Sábado)
05 09 20 25 35 53
Concurso: 2191 - 24/09/2019 (Terça)
04 08 26 33 46 53
Concurso: 2192 - 26/09/2019 (Quinta)
07 16 37 53 57 59
Concurso: 2193 - 28/09/2019 (Sábado)
07 08 22 27 29 42
Outubro/2019

Concurso: 2194 - 02/10/2019 (Quarta)
08 16 20 21 31 34
Concurso: 2195 - 05/10/2019 (Sábado)
14 24 32 38 46 53
Concurso: 2196 - 09/10/2019 (Quarta)
01 25 27 28 41 56
Concurso: 2197 - 14/10/2019 (Segunda)
03 11 29 35 44 57
Concurso: 2198 - 16/10/2019 (Quarta)
01 11 34 36 44 56
Concurso: 2199 - 19/10/2019 (Sábado)
15 23 30 35 38 44
Concurso: 2200 - 22/10/2019 (Terça)
11 15 28 36 43 55
Concurso: 2201 - 24/10/2019 (Quinta)
03 13 23 41 49 53
Concurso: 2202 - 26/10/2019 (Sábado)
11 29 37 38 43 60
Concurso: 2203 - 30/10/2019 (Quarta)
17 34 46 49 50 57
Novembro/2019

Concurso: 2204 - 04/11/2019 (Segunda)
01 28 29 32 35 56
Concurso: 2205 - 06/11/2019 (Quarta)
12 21 28 37 42 57
Concurso: 2206 - 09/11/2019 (Sábado)
06 27 38 42 45 57
Concurso: 2207 - 13/11/2019 (Quarta)
06 10 11 43 53 55
Concurso: 2208 - 16/11/2019 (Sábado)
16 25 30 40 45 49
Concurso: 2209 - 20/11/2019 (Quarta)
22 25 28 32 33 47
Concurso: 2210 - 23/11/2019 (Sábado)
11 17 24 25 33 34
Concurso: 2211 - 27/11/2019 (Quarta)
27 36 40 41 44 54
Concurso: 2212 - 30/11/2019 (Sábado)
23 26 51 52 53 58
Dezembro/2019

Concurso: 2213 - 04/12/2019 (Quarta)
05 07 10 32 46 60
Concurso: 2214 - 07/12/2019 (Sábado)
04 10 18 30 34 47
Concurso: 2215 - 11/12/2019 (Quarta)
01 19 21 23 33 43
Concurso: 2216 - 14/12/2019 (Sábado)
10 24 42 43 48 49
Concurso: 2217 - 17/12/2019 (Terça)
10 14 16 30 32 36
Concurso: 2218 - 19/12/2019 (Quinta)
06 16 22 38 48 52
Concurso: 2219 - 21/12/2019 (Sábado)
08 28 36 45 57 59
Concurso: 2220 - 31/12/2019 (Terça)
03 35 38 40 57 58
Janeiro/2020
Concurso: 2221 - 04/01/2020 (Sábado)
05 23 34 45 56 57
Concurso: 2222 - 08/01/2020 (Quarta)
13 14 29 30 48 59
Concurso: 2223 - 11/01/2020 (Sábado)
02 26 40 42 49 56
Concurso: 2224 - 15/01/2020 (Quarta)
16 23 32 50 52 58
Concurso: 2225 - 18/01/2020 (Sábado)
01 32 37 44 46 47
Concurso: 2226 - 21/01/2020 (Terça)
02 04 07 16 30 38
Concurso: 2227 - 23/01/2020 (Quinta)
06 09 12 27 32 57
Concurso: 2228 - 25/01/2020 (Sábado)
09 19 23 32 39 45
Concurso: 2229 - 29/01/2020 (Quarta)
06 11 29 40 41 58
Fevereiro/2020
Concurso: 2230 - 01/02/2020 (Sábado)
07 17 26 39 56 60
Concurso: 2231 - 05/02/2020 (Quarta)
04 13 25 40 53 57
Concurso: 2232 - 08/02/2020 (Sábado)
07 08 31 34 38 47
Concurso: 2233 - 12/02/2020 (Quarta)
04 06 32 35 41 45
Concurso: 2234 - 15/02/2020 (Sábado)
04 21 27 29 42 47
Concurso: 2235 - 19/02/2020 (Quarta)
14 18 30 35 55 57
Concurso: 2236 - 22/02/2020 (Sábado)
07 20 38 43 45 53
Concurso: 2237 - 27/02/2020 (Quinta)
11 20 27 28 53 60
Concurso: 2238 - 29/02/2020 (Sábado)
11 36 45 55 57 58
Março/2020
Concurso: 2239 - 04/03/2020 (Quarta)
07 27 31 39 45 46
Concurso: 2240 - 07/03/2020 (Sábado)
07 09 10 19 25 58
Concurso: 2241 - 10/03/2020 (Terça)
01 13 18 26 40 56
Concurso: 2242 - 12/03/2020 (Quinta)
05 09 18 24 25 42
Concurso: 2243 - 14/03/2020 (Sábado)
14 18 28 35 38 54
Concurso: 2244 - 18/03/2020 (Quarta)
03 05 11 34 37 42
Concurso: 2245 - 21/03/2020 (Sábado)
11 14 15 18 33 34
Concurso: 2246 - 25/03/2020 (Quarta)
05 09 24 27 33 46
Concurso: 2247 - 28/03/2020 (Sábado)
01 42 44 47 48 53
Abril/2020
Concurso: 2248 - 01/04/2020 (Quarta)
09 15 20 29 30 42
Concurso: 2249 - 04/04/2020 (Sábado)
04 09 31 47 49 53
Concurso: 2250 - 08/04/2020 (Quarta)
27 33 39 52 57 58
Concurso: 2251 - 11/04/2020 (Sábado)
05 15 22 26 54 58
Concurso: 2252 - 15/04/2020 (Quarta)
01 17 30 37 46 50
Concurso: 2253 - 18/04/2020 (Sábado)
31 32 33 39 52 55
Concurso: 2254 - 22/04/2020 (Quarta)
04 09 24 44 47 56
Concurso: 2255 - 25/04/2020 (Sábado)
15 20 39 41 49 57
Concurso: 2256 - 29/04/2020 (Quarta)
09 10 30 37 47 54
Maio/2020
Concurso: 2257 - 02/05/2020 (Sábado)
18 21 30 31 34 51
Concurso: 2258 - 05/05/2020 (Terça)
01 05 07 14 23 26
Concurso: 2259 - 07/05/2020 (Quinta)
20 27 41 54 56 58
Concurso: 2260 - 09/05/2020 (Sábado)
12 14 34 35 37 47
Concurso: 2261 - 13/05/2020 (Quarta)
07 23 26 27 29 51
Concurso: 2262 - 16/05/2020 (Sábado)
07 08 14 23 30 46
Concurso: 2263 - 20/05/2020 (Quarta)
09 24 33 43 49 57
Concurso: 2264 - 23/05/2020 (Sábado)
02 03 08 19 29 37
Concurso: 2265 - 27/05/2020 (Quarta)
14 20 23 39 46 50
Concurso: 2266 - 30/05/2020 (Sábado)
10 23 31 37 58 59
Junho/2020
Concurso: 2267 - 03/06/2020 (Quarta)
20 32 33 48 49 53
Concurso: 2268 - 06/06/2020 (Sábado)
04 13 23 28 30 52
Concurso: 2269 - 10/06/2020 (Quarta)
01 11 14 23 29 55
Concurso: 2270 - 13/06/2020 (Sábado)
14 16 38 39 41 48
Concurso: 2271 - 17/06/2020 (Quarta)
06 10 27 28 41 52
Concurso: 2272 - 20/06/2020 (Sábado)
02 05 11 24 41 49
Concurso: 2273 - 24/06/2020 (Quarta)
15 16 20 38 40 58
Concurso: 2274 - 27/06/2020 (Sábado)
08 11 17 33 40 55
Julho/2020
Concurso: 2275 - 01/07/2020 (Quarta)
02 04 25 36 50 53
Concurso: 2276 - 04/07/2020 (Sábado)
05 15 18 27 49 57
Concurso: 2277 - 08/07/2020 (Quarta)
10 22 23 37 53 60
Concurso: 2278 - 11/07/2020 (Sábado)
08 17 34 37 43 45
Concurso: 2279 - 14/07/2020 (Terça)
05 12 14 20 27 28
Concurso: 2280 - 16/07/2020 (Quinta)
28 29 31 50 58 59
Concurso: 2281 - 18/07/2020 (Sábado)
14 27 35 40 50 55
Concurso: 2282 - 22/07/2020 (Quarta)
12 27 30 36 45 52
Concurso: 2283 - 25/07/2020 (Sábado)
04 24 37 43 59 60
Concurso: 2284 - 29/07/2020 (Quarta)
04 10 12 14 36 46
Agosto/2020
Concurso: 2285 - 01/08/2020 (Sábado)
01 07 10 12 33 42
Concurso: 2286 - 05/08/2020 (Quarta)
09 21 30 41 42 43
Concurso: 2287 - 08/08/2020 (Sábado)
02 04 06 29 41 56
Concurso: 2288 - 11/08/2020 (Terça)
02 26 35 39 40 56
Concurso: 2289 - 13/08/2020 (Quinta)
06 09 34 37 38 45
Concurso: 2290 - 15/08/2020 (Sábado)
05 18 36 44 57 60
Concurso: 2291 - 19/08/2020 (Quarta)
12 26 31 36 37 49
Concurso: 2292 - 22/08/2020 (Sábado)
06 16 18 33 42 57
Concurso: 2293 - 26/08/2020 (Quarta)
01 02 10 37 42 48
Concurso: 2294 - 29/08/2020 (Sábado)
09 15 20 33 41 43
Setembro/2020
Concurso: 2295 - 02/09/2020 (Quarta)
06 13 26 28 35 41
Concurso: 2296 - 05/09/2020 (Sábado)
01 06 21 29 36 59
Concurso: 2297 - 09/09/2020 (Quarta)
20 22 35 40 41 59
Concurso: 2298 - 12/09/2020 (Sábado)
13 17 21 31 41 49
Concurso: 2299 - 15/09/2020 (Terça)
02 03 19 40 44 60
Concurso: 2300 - 17/09/2020 (Quinta)
09 21 37 39 43 54
Concurso: 2301 - 19/09/2020 (Sábado)
17 18 35 36 47 52
Concurso: 2302 - 23/09/2020 (Quarta)
18 22 25 27 43 44
Concurso: 2303 - 26/09/2020 (Sábado)
03 07 17 20 48 50
Concurso: 2304 - 30/09/2020 (Quarta)
12 21 29 54 56 57
Outubro/2020
Concurso: 2305 - 03/10/2020 (Sábado)
07 16 22 38 55 57
Concurso: 2306 - 07/10/2020 (Quarta)
03 19 28 33 57 58
Concurso: 2307 - 10/10/2020 (Sábado)
16 33 38 46 53 55
Concurso: 2308 - 14/10/2020 (Quarta)
09 13 26 38 58 60
Concurso: 2309 - 17/10/2020 (Sábado)
09 11 29 30 33 60
Concurso: 2310 - 20/10/2020 (Terça)
13 17 28 29 42 53
Concurso: 2311 - 22/10/2020 (Quinta)
03 05 09 35 43 60
Concurso: 2312 - 24/10/2020 (Sábado)
03 27 39 46 47 60
Concurso: 2313 - 28/10/2020 (Quarta)
03 20 26 45 49 58
Concurso: 2314 - 31/10/2020 (Sábado)
06 07 28 42 45 49
Novembro/2020
Concurso: 2315 - 04/11/2020 (Quarta)
01 10 17 26 30 53
Concurso: 2316 - 07/11/2020 (Sábado)
02 11 43 49 52 56
Concurso: 2317 - 11/11/2020 (Quarta)
03 08 30 33 35 48
Concurso: 2318 - 14/11/2020 (Sábado)
28 44 52 54 58 60
Concurso: 2319 - 18/11/2020 (Quarta)
06 17 25 35 40 49
Concurso: 2320 - 21/11/2020 (Sábado)
06 30 35 39 42 48
Concurso: 2321 - 25/11/2020 (Quarta)
14 25 28 41 43 46
Concurso: 2322 - 28/11/2020 (Sábado)
02 05 10 29 34 41
Dezembro/2020
Concurso: 2323 - 02/12/2020 (Quarta)
20 27 35 39 50 59
Concurso: 2324 - 05/12/2020 (Sábado)
02 16 19 31 43 60
Concurso: 2325 - 08/12/2020 (Terça)
33 34 37 46 52 60
Concurso: 2326 - 10/12/2020 (Quinta)
10 16 27 34 36 57
Concurso: 2327 - 12/12/2020 (Sábado)
30 37 45 54 57 58
Concurso: 2328 - 16/12/2020 (Quarta)
08 11 14 39 48 53
Concurso: 2329 - 19/12/2020 (Sábado)
12 14 28 42 45 55
Concurso: 2330 - 31/12/2020 (Quinta)
17 20 22 35 41 42
Janeiro/2021
Concurso: 2331 - 02/01/2021 (Sábado)
11 13 16 36 53 57
Concurso: 2332 - 06/01/2021 (Quarta)
12 33 35 36 44 52
Concurso: 2333 - 09/01/2021 (Sábado)
09 16 31 41 53 55
Concurso: 2334 - 13/01/2021 (Quarta)
04 13 20 22 25 60
Concurso: 2335 - 16/01/2021 (Sábado)
09 18 23 42 47 49
Concurso: 2336 - 20/01/2021 (Quarta)
08 10 20 27 28 50
Concurso: 2337 - 23/01/2021 (Sábado)
02 09 34 49 51 55
Concurso: 2338 - 26/01/2021 (Terça)
08 21 23 34 42 47
Concurso: 2339 - 28/01/2021 (Quinta)
04 18 29 47 48 59
Concurso: 2340 - 30/01/2021 (Sábado)
16 21 28 41 49 51
Fevereiro/2021
Concurso: 2341 - 03/02/2021 (Quarta)
04 09 31 32 42 46
Concurso: 2342 - 06/02/2021 (Sábado)
17 20 24 27 40 60
Concurso: 2343 - 10/02/2021 (Quarta)
04 31 42 45 49 56
Concurso: 2344 - 13/02/2021 (Sábado)
11 17 25 38 52 57
Concurso: 2345 - 17/02/2021 (Quarta)
07 16 19 22 28 55
Concurso: 2346 - 20/02/2021 (Sábado)
03 04 11 40 42 58
Concurso: 2347 - 24/02/2021 (Quarta)
08 09 17 30 58 60
Concurso: 2348 - 27/02/2021 (Sábado)
02 03 07 48 51 54
Março/2021
Concurso: 2349 - 03/03/2021 (Quarta)
05 10 25 32 49 54
Concurso: 2350 - 06/03/2021 (Sábado)
25 28 29 34 41 45
Concurso: 2351 - 10/03/2021 (Quarta)
19 22 35 41 47 49
Concurso: 2352 - 13/03/2021 (Sábado)
09 17 38 41 49 55
Concurso: 2353 - 17/03/2021 (Quarta)
03 19 34 41 48 53
Concurso: 2354 - 20/03/2021 (Sábado)
06 18 25 30 42 54
Concurso: 2355 - 24/03/2021 (Quarta)
07 30 31 41 50 56
Concurso: 2356 - 27/03/2021 (Sábado)
03 10 25 36 51 58
Concurso: 2357 - 31/03/2021 (Quarta)
19 28 30 34 40 51
Abril/2021
Concurso: 2358 - 03/04/2021 (Sábado)
05 09 11 16 43 57
Concurso: 2359 - 06/04/2021 (Terça)
31 32 39 42 43 51
Concurso: 2360 - 08/04/2021 (Quinta)
10 15 21 24 29 45
Concurso: 2361 - 10/04/2021 (Sábado)
14 21 22 29 35 46
Concurso: 2362 - 14/04/2021 (Quarta)
03 20 22 32 35 50
Concurso: 2363 - 17/04/2021 (Sábado)
06 14 24 34 39 58
Concurso: 2364 - 22/04/2021 (Quinta)
20 33 42 44 51 56
Concurso: 2365 - 24/04/2021 (Sábado)
01 17 28 37 44 50
Concurso: 2366 - 28/04/2021 (Quarta)
04 27 33 35 38 41
Concurso: 2367 - 30/04/2021 (Sexta)
05 23 29 34 53 60
Maio/2021
Concurso: 2368 - 04/05/2021 (Terça)
04 07 13 25 36 58
Concurso: 2369 - 06/05/2021 (Quinta)
04 09 17 19 37 60
Concurso: 2370 - 08/05/2021 (Sábado)
07 31 37 42 44 56
Concurso: 2371 - 12/05/2021 (Quarta)
04 15 30 36 39 48
Concurso: 2372 - 15/05/2021 (Sábado)
03 19 25 44 46 57
Concurso: 2373 - 19/05/2021 (Quarta)
23 24 26 44 49 60
Concurso: 2374 - 22/05/2021 (Sábado)
12 13 25 37 39 41
Concurso: 2375 - 26/05/2021 (Quarta)
02 06 44 46 53 58
Concurso: 2376 - 29/05/2021 (Sábado)
12 14 17 18 19 22
Junho/2021
Concurso: 2377 - 02/06/2021 (Quarta)
05 18 29 35 43 44
Concurso: 2378 - 05/06/2021 (Sábado)
11 37 38 41 49 54
Concurso: 2379 - 09/06/2021 (Quarta)
02 08 26 32 46 56
Concurso: 2380 - 12/06/2021 (Sábado)
11 16 20 24 39 53
Concurso: 2381 - 16/06/2021 (Quarta)
07 23 32 41 42 47
Concurso: 2382 - 19/06/2021 (Sábado)
06 09 19 38 53 55
Concurso: 2383 - 23/06/2021 (Quarta)
13 15 16 20 40 41
Concurso: 2384 - 26/06/2021 (Sábado)
09 13 22 25 26 31
Concurso: 2385 - 29/06/2021 (Terça)
12 24 32 37 43 60
Julho/2021
Concurso: 2386 - 01/07/2021 (Quinta)
11 13 16 35 49 50
Concurso: 2387 - 03/07/2021 (Sábado)
08 26 30 31 38 48
Concurso: 2388 - 07/07/2021 (Quarta)
08 30 33 37 45 48
Concurso: 2389 - 10/07/2021 (Sábado)
16 30 37 39 40 51
Concurso: 2390 - 14/07/2021 (Quarta)
09 13 20 22 32 56
Concurso: 2391 - 17/07/2021 (Sábado)
05 08 13 27 36 50
Concurso: 2392 - 21/07/2021 (Quarta)
11 15 23 25 34 53
Concurso: 2393 - 24/07/2021 (Sábado)
26 27 28 32 38 51
Concurso: 2394 - 28/07/2021 (Quarta)
05 16 25 36 42 44
Concurso: 2395 - 31/07/2021 (Sábado)
04 11 12 44 45 57
Agosto/2021
Concurso: 2396 - 04/08/2021 (Quarta)
02 03 25 39 42 49
Concurso: 2397 - 07/08/2021 (Sábado)
06 14 20 39 46 48
Concurso: 2398 - 10/08/2021 (Terça)
08 17 30 36 54 59
Concurso: 2399 - 12/08/2021 (Quinta)
27 35 42 44 51 52
Concurso: 2400 - 14/08/2021 (Sábado)
09 21 25 26 36 53
Concurso: 2401 - 18/08/2021 (Quarta)
08 11 13 33 38 48
Concurso: 2402 - 21/08/2021 (Sábado)
06 22 25 29 30 60
Concurso: 2403 - 25/08/2021 (Quarta)
10 12 14 32 33 34
Concurso: 2404 - 28/08/2021 (Sábado)
01 19 35 40 47 54
Setembro/2021
Concurso: 2405 - 01/09/2021 (Quarta)
21 38 48 49 53 59
Concurso: 2406 - 04/09/2021 (Sábado)
08 12 29 43 54 60
Concurso: 2407 - 08/09/2021 (Quarta)
13 17 31 43 54 55
Concurso: 2408 - 11/09/2021 (Sábado)
04 29 30 38 43 57
Concurso: 2409 - 15/09/2021 (Quarta)
02 29 39 49 52 58
Concurso: 2410 - 18/09/2021 (Sábado)
07 10 27 35 43 59
Concurso: 2411 - 22/09/2021 (Quarta)
07 26 29 34 43 44
Concurso: 2412 - 25/09/2021 (Sábado)
09 16 34 36 49 60
Concurso: 2413 - 28/09/2021 (Terça)
03 22 37 40 41 48
Concurso: 2414 - 30/09/2021 (Quinta)
04 05 06 14 29 38
Outubro/2021
Concurso: 2415 - 02/10/2021 (Sábado)
10 12 26 29 35 60
Concurso: 2416 - 06/10/2021 (Quarta)
06 07 11 26 37 57
Concurso: 2417 - 09/10/2021 (Sábado)
03 07 10 11 27 46
Concurso: 2418 - 13/10/2021 (Quarta)
02 11 19 27 57 60
Concurso: 2419 - 16/10/2021 (Sábado)
10 35 43 48 50 53
Concurso: 2420 - 19/10/2021 (Terça)
05 08 29 39 44 60
Concurso: 2421 - 21/10/2021 (Quinta)
02 03 32 35 48 57
Concurso: 2422 - 23/10/2021 (Sábado)
02 07 10 20 30 46
Concurso: 2423 - 27/10/2021 (Quarta)
16 18 38 48 51 60
Concurso: 2424 - 30/10/2021 (Sábado)
03 16 17 37 38 53
Novembro/2021
Concurso: 2425 - 03/11/2021 (Quarta)
10 31 38 46 49 54
Concurso: 2426 - 06/11/2021 (Sábado)
05 11 24 27 32 57
Concurso: 2427 - 10/11/2021 (Quarta)
03 19 25 37 44 56
Concurso: 2428 - 13/11/2021 (Sábado)
03 09 25 28 29 39
Concurso: 2429 - 17/11/2021 (Quarta)
11 37 53 55 56 60
Concurso: 2430 - 20/11/2021 (Sábado)
19 26 39 45 46 56
Concurso: 2431 - 24/11/2021 (Quarta)
08 11 22 25 26 36
Concurso: 2432 - 27/11/2021 (Sábado)
07 29 38 40 44 52
Dezembro/2021
Concurso: 2433 - 01/12/2021 (Quarta)
08 09 32 52 53 57
Concurso: 2434 - 04/12/2021 (Sábado)
01 02 14 28 40 51
Concurso: 2435 - 07/12/2021 (Terça)
05 22 30 32 33 36
Concurso: 2436 - 09/12/2021 (Quinta)
05 15 28 32 38 54
Concurso: 2437 - 11/12/2021 (Sábado)
01 19 41 46 48 55
Concurso: 2438 - 15/12/2021 (Quarta)
04 11 19 25 37 55
Concurso: 2439 - 18/12/2021 (Sábado)
02 08 34 38 47 51
Concurso: 2440 - 31/12/2021 (Sexta)
12 15 23 32 33 46
Janeiro/2022
Concurso: 2441 - 05/01/2022 (Quarta)
09 41 42 46 47 54
Concurso: 2442 - 08/01/2022 (Sábado)
02 07 09 25 41 49
Concurso: 2443 - 12/01/2022 (Quarta)
01 05 12 13 17 31
Concurso: 2444 - 15/01/2022 (Sábado)
15 17 20 35 37 43
Concurso: 2445 - 19/01/2022 (Quarta)
11 25 32 37 47 56
Concurso: 2446 - 22/01/2022 (Sábado)
01 13 27 41 51 58
Concurso: 2447 - 25/01/2022 (Terça)
13 19 29 42 49 52
Concurso: 2448 - 27/01/2022 (Quinta)
18 30 32 35 40 48
Concurso: 2449 - 29/01/2022 (Sábado)
14 20 21 31 49 52
Fevereiro/2022
Concurso: 2450 - 02/02/2022 (Quarta)
02 06 11 15 17 39
Concurso: 2451 - 05/02/2022 (Sábado)
13 26 31 46 51 60
Concurso: 2452 - 09/02/2022 (Quarta)
08 10 51 56 57 58
Concurso: 2453 - 12/02/2022 (Sábado)
10 14 15 24 34 44
Concurso: 2454 - 16/02/2022 (Quarta)
09 14 22 24 44 47
Concurso: 2455 - 19/02/2022 (Sábado)
21 38 50 53 56 59
Concurso: 2456 - 22/02/2022 (Terça)
28 34 40 41 52 55
Concurso: 2457 - 24/02/2022 (Quinta)
10 19 46 47 49 50
Concurso: 2458 - 26/02/2022 (Sábado)
15 40 44 45 47 51
Março/2022
Concurso: 2459 - 03/03/2022 (Quinta)
15 24 33 49 53 59
Concurso: 2460 - 05/03/2022 (Sábado)
16 17 18 28 35 47
Concurso: 2461 - 09/03/2022 (Quarta)
08 11 16 21 32 37
Concurso: 2462 - 12/03/2022 (Sábado)
03 16 23 41 45 57
Concurso: 2463 - 16/03/2022 (Quarta)
11 16 31 37 42 51
Concurso: 2464 - 19/03/2022 (Sábado)
02 07 24 43 52 56
Concurso: 2465 - 23/03/2022 (Quarta)
03 08 23 29 53 54
Concurso: 2466 - 26/03/2022 (Sábado)
02 03 13 20 53 54
Concurso: 2467 - 30/03/2022 (Quarta)
01 10 19 34 35 45
Abril/2022
Concurso: 2468 - 02/04/2022 (Sábado)
22 35 41 42 53 57
Concurso: 2469 - 06/04/2022 (Quarta)
05 28 30 38 52 55
Concurso: 2470 - 09/04/2022 (Sábado)
08 33 40 42 48 51
Concurso: 2471 - 13/04/2022 (Quarta)
08 23 29 30 36 55
Concurso: 2472 - 16/04/2022 (Sábado)
05 13 18 23 35 36
Concurso: 2473 - 20/04/2022 (Quarta)
15 18 28 42 55 60
Concurso: 2474 - 23/04/2022 (Sábado)
22 30 38 39 49 56
Concurso: 2475 - 26/04/2022 (Terça)
01 40 44 45 58 60
Concurso: 2476 - 28/04/2022 (Quinta)
02 07 32 46 49 55
Concurso: 2477 - 30/04/2022 (Sábado)
20 33 37 38 49 50
Maio/2022
Concurso: 2478 - 04/05/2022 (Quarta)
02 17 23 28 39 46
Concurso: 2479 - 07/05/2022 (Sábado)
10 15 17 20 21 35
Concurso: 2480 - 11/05/2022 (Quarta)
04 06 09 31 50 56
Concurso: 2481 - 14/05/2022 (Sábado)
01 08 21 27 36 37
Concurso: 2482 - 18/05/2022 (Quarta)
01 32 35 44 45 57
Concurso: 2483 - 21/05/2022 (Sábado)
20 34 38 40 49 54
Concurso: 2484 - 25/05/2022 (Quarta)
11 14 36 41 54 59
Concurso: 2485 - 28/05/2022 (Sábado)
05 12 32 38 47 60
Concurso: 2486 - 31/05/2022 (Terça)
08 09 17 19 33 56
Junho/2022
Concurso: 2487 - 02/06/2022 (Quinta)
23 36 42 48 54 58
Concurso: 2488 - 04/06/2022 (Sábado)
17 31 34 40 56 57
Concurso: 2489 - 08/06/2022 (Quarta)
03 10 13 25 41 42
Concurso: 2490 - 11/06/2022 (Sábado)
11 16 17 41 46 59
Concurso: 2491 - 15/06/2022 (Quarta)
22 29 38 43 48 53
Concurso: 2492 - 18/06/2022 (Sábado)
10 30 31 33 42 52
Concurso: 2493 - 22/06/2022 (Quarta)
04 09 37 43 44 56
Concurso: 2494 - 25/06/2022 (Sábado)
01 04 10 22 53 54
Concurso: 2495 - 28/06/2022 (Terça)
08 12 14 30 33 41
Concurso: 2496 - 30/06/2022 (Quinta)
07 26 31 38 46 58
Julho/2022
Concurso: 2497 - 02/07/2022 (Sábado)
05 14 23 46 48 52
Concurso: 2498 - 06/07/2022 (Quarta)
09 12 26 29 46 47
Concurso: 2499 - 09/07/2022 (Sábado)
11 19 38 47 56 59
Concurso: 2500 - 13/07/2022 (Quarta)
05 16 25 32 39 55
Concurso: 2501 - 16/07/2022 (Sábado)
11 27 32 40 58 59
Concurso: 2502 - 20/07/2022 (Quarta)
16 20 21 39 44 55
Concurso: 2503 - 23/07/2022 (Sábado)
03 14 16 38 43 45
Concurso: 2504 - 27/07/2022 (Quarta)
14 33 41 42 44 55
Concurso: 2505 - 30/07/2022 (Sábado)
03 05 19 26 43 51
Agosto/2022
Concurso: 2506 - 02/08/2022 (Terça)
21 22 29 34 40 44
Concurso: 2507 - 04/08/2022 (Quinta)
04 06 12 34 35 53
Concurso: 2508 - 06/08/2022 (Sábado)
41 45 48 51 53 58
Concurso: 2509 - 10/08/2022 (Quarta)
08 37 39 50 59 60
Concurso: 2510 - 13/08/2022 (Sábado)
08 13 25 32 44 57
Concurso: 2511 - 17/08/2022 (Quarta)
04 10 15 39 41 49
Concurso: 2512 - 20/08/2022 (Sábado)
07 10 34 47 49 52
Concurso: 2513 - 24/08/2022 (Quarta)
13 19 21 35 46 50
Concurso: 2514 - 27/08/2022 (Sábado)
05 15 24 34 45 52
Concurso: 2515 - 31/08/2022 (Quarta)
03 12 19 41 45 54
Setembro/2022
Concurso: 2516 - 03/09/2022 (Sábado)
08 17 49 51 52 53
Concurso: 2517 - 08/09/2022 (Quinta)
01 05 06 16 22 39
Concurso: 2518 - 10/09/2022 (Sábado)
03 22 23 44 53 60
Concurso: 2519 - 13/09/2022 (Terça)
03 08 20 36 38 57
Concurso: 2520 - 15/09/2022 (Quinta)
02 17 22 41 58 60
Concurso: 2521 - 17/09/2022 (Sábado)
23 28 33 38 55 59
Concurso: 2522 - 21/09/2022 (Quarta)
04 05 25 32 39 40
Concurso: 2523 - 24/09/2022 (Sábado)
01 10 27 36 37 45
Concurso: 2524 - 28/09/2022 (Quarta)
03 20 22 37 41 43
Outubro/2022
Concurso: 2525 - 01/10/2022 (Sábado)
04 13 21 26 47 51
Concurso: 2526 - 05/10/2022 (Quarta)
02 16 24 38 43 59
Concurso: 2527 - 08/10/2022 (Sábado)
08 19 29 38 48 56
Concurso: 2528 - 13/10/2022 (Quinta)
04 15 22 53 56 60
Concurso: 2529 - 15/10/2022 (Sábado)
03 05 32 56 57 59
Concurso: 2530 - 18/10/2022 (Terça)
14 17 18 28 30 44
Concurso: 2531 - 20/10/2022 (Quinta)
01 05 18 49 55 56
Concurso: 2532 - 22/10/2022 (Sábado)
10 14 17 18 23 30
Concurso: 2533 - 26/10/2022 (Quarta)
17 18 20 37 45 53
Concurso: 2534 - 29/10/2022 (Sábado)
28 36 39 44 56 60
Novembro/2022
Concurso: 2535 - 03/11/2022 (Quinta)
01 03 24 37 51 56
Concurso: 2536 - 05/11/2022 (Sábado)
09 22 27 30 33 45
Concurso: 2537 - 09/11/2022 (Quarta)
12 24 26 31 37 48
Concurso: 2538 - 12/11/2022 (Sábado)
06 15 19 20 33 52
Concurso: 2539 - 16/11/2022 (Quarta)
01 23 32 33 36 59
Concurso: 2540 - 19/11/2022 (Sábado)
02 08 28 34 41 49
Concurso: 2541 - 22/11/2022 (Terça)
10 28 45 47 57 59
Concurso: 2542 - 24/11/2022 (Quinta)
12 20 22 25 26 55
Concurso: 2543 - 26/11/2022 (Sábado)
02 05 27 30 46 53
Concurso: 2544 - 30/11/2022 (Quarta)
25 38 45 53 55 56
Dezembro/2022
Concurso: 2545 - 03/12/2022 (Sábado)
20 23 32 36 39 57
Concurso: 2546 - 07/12/2022 (Quarta)
03 23 28 34 38 48
Concurso: 2547 - 10/12/2022 (Sábado)
10 25 31 37 38 57
Concurso: 2548 - 14/12/2022 (Quarta)
09 15 23 25 29 30
Concurso: 2549 - 17/12/2022 (Sábado)
01 06 10 30 33 35
Concurso: 2550 - 31/12/2022 (Sábado)
04 05 10 34 58 59
Janeiro/2023
Concurso: 2551 - 04/01/2023 (Quarta)
01 25 29 43 46 48
Concurso: 2552 - 07/01/2023 (Sábado)
05 24 25 33 38 41
Concurso: 2553 - 10/01/2023 (Terça)
13 15 53 54 55 58
Concurso: 2554 - 12/01/2023 (Quinta)
19 25 43 44 48 49
Concurso: 2555 - 14/01/2023 (Sábado)
03 20 45 52 53 58
Concurso: 2556 - 18/01/2023 (Quarta)
02 06 10 14 34 56
Concurso: 2557 - 21/01/2023 (Sábado)
03 13 16 25 27 33
Concurso: 2558 - 25/01/2023 (Quarta)
02 10 18 25 34 44
Concurso: 2559 - 28/01/2023 (Sábado)
09 12 20 30 32 35
Fevereiro/2023
Concurso: 2560 - 01/02/2023 (Quarta)
04 05 17 20 48 52
Concurso: 2561 - 04/02/2023 (Sábado)
19 22 37 44 51 56
Concurso: 2562 - 08/02/2023 (Quarta)
06 12 32 44 51 57
Concurso: 2563 - 11/02/2023 (Sábado)
05 09 14 30 38 50
Concurso: 2564 - 14/02/2023 (Terça)
07 08 14 19 32 45
Concurso: 2565 - 16/02/2023 (Quinta)
09 13 25 39 46 54
Concurso: 2566 - 18/02/2023 (Sábado)
11 23 45 53 57 59
Concurso: 2567 - 23/02/2023 (Quinta)
10 11 19 33 58 60
Concurso: 2568 - 25/02/2023 (Sábado)
16 22 29 35 38 49
Março/2023
Concurso: 2569 - 01/03/2023 (Quarta)
06 07 25 28 31 52
Concurso: 2570 - 04/03/2023 (Sábado)
08 18 26 27 47 50
Concurso: 2571 - 08/03/2023 (Quarta)
09 18 33 38 41 51
Concurso: 2572 - 11/03/2023 (Sábado)
03 07 15 22 24 50
Concurso: 2573 - 14/03/2023 (Terça)
06 26 32 35 37 49
Concurso: 2574 - 16/03/2023 (Quinta)
12 17 43 44 48 60
Concurso: 2575 - 18/03/2023 (Sábado)
04 12 14 41 46 53
Concurso: 2576 - 22/03/2023 (Quarta)
29 32 33 35 38 43
Concurso: 2577 - 25/03/2023 (Sábado)
12 18 22 31 44 50
Concurso: 2578 - 29/03/2023 (Quarta)
37 39 47 50 59 60
Abril/2023
Concurso: 2579 - 01/04/2023 (Sábado)
05 10 26 35 38 44
Concurso: 2580 - 05/04/2023 (Quarta)
03 04 13 29 36 43
Concurso: 2581 - 08/04/2023 (Sábado)
14 17 32 36 39 60
Concurso: 2582 - 12/04/2023 (Quarta)
10 14 17 19 21 34
Concurso: 2583 - 15/04/2023 (Sábado)
02 20 27 30 52 59
Concurso: 2584 - 19/04/2023 (Quarta)
01 05 12 36 53 55
Concurso: 2585 - 22/04/2023 (Sábado)
14 26 34 36 43 59
Concurso: 2586 - 26/04/2023 (Quarta)
10 18 41 49 53 59
Concurso: 2587 - 29/04/2023 (Sábado)
05 10 11 22 23 37
Maio/2023
Concurso: 2588 - 03/05/2023 (Quarta)
09 13 22 31 57 58
Concurso: 2589 - 06/05/2023 (Sábado)
01 15 16 25 32 36
Concurso: 2590 - 09/05/2023 (Terça)
02 12 28 36 43 48
Concurso: 2591 - 11/05/2023 (Quinta)
10 11 21 23 28 30
Concurso: 2592 - 13/05/2023 (Sábado)
15 17 28 34 35 51
Concurso: 2593 - 17/05/2023 (Quarta)
10 14 17 25 32 39
Concurso: 2594 - 20/05/2023 (Sábado)
07 26 32 35 49 55
Concurso: 2595 - 24/05/2023 (Quarta)
01 13 34 39 50 52
Concurso: 2596 - 27/05/2023 (Sábado)
34 35 39 47 51 56
Concurso: 2597 - 31/05/2023 (Quarta)
14 26 34 54 56 58
Junho/2023
Concurso: 2598 - 03/06/2023 (Sábado)
07 14 24 53 58 60
Concurso: 2599 - 07/06/2023 (Quarta)
23 28 34 43 47 60
Concurso: 2600 - 10/06/2023 (Sábado)
04 18 37 38 46 60
Concurso: 2601 - 14/06/2023 (Quarta)
03 08 34 40 44 55
Concurso: 2602 - 17/06/2023 (Sábado)
11 14 16 30 32 46
Concurso: 2603 - 21/06/2023 (Quarta)
03 07 13 29 52 56
Concurso: 2604 - 24/06/2023 (Sábado)
16 17 19 22 46 57
Concurso: 2605 - 27/06/2023 (Terça)
05 11 26 35 46 54
Concurso: 2606 - 29/06/2023 (Quinta)
02 10 16 32 45 49
Julho/2023
Concurso: 2607 - 01/07/2023 (Sábado)
07 11 25 51 57 60
Concurso: 2608 - 05/07/2023 (Quarta)
07 13 17 24 29 52
Concurso: 2609 - 08/07/2023 (Sábado)
03 21 27 32 35 60
Concurso: 2610 - 12/07/2023 (Quarta)
10 23 34 53 55 57
Concurso: 2611 - 15/07/2023 (Sábado)
04 12 18 21 25 49
Concurso: 2612 - 19/07/2023 (Quarta)
20 27 34 44 50 54
Concurso: 2613 - 22/07/2023 (Sábado)
14 26 40 42 46 52
Concurso: 2614 - 25/07/2023 (Terça)
03 08 13 14 19 25
Concurso: 2615 - 27/07/2023 (Quinta)
05 07 22 23 41 59
Concurso: 2616 - 29/07/2023 (Sábado)
06 16 23 35 38 49
Agosto/2023
Concurso: 2617 - 02/08/2023 (Quarta)
03 14 36 42 43 44
Concurso: 2618 - 05/08/2023 (Sábado)
06 17 29 35 45 48
Concurso: 2619 - 09/08/2023 (Quarta)
05 36 39 41 44 50
Concurso: 2620 - 12/08/2023 (Sábado)
04 06 13 21 26 28
Concurso: 2621 - 16/08/2023 (Quarta)
06 09 14 16 42 47
Concurso: 2622 - 19/08/2023 (Sábado)
09 19 22 24 50 60
Concurso: 2623 - 22/08/2023 (Terça)
10 15 20 35 37 59
Concurso: 2624 - 24/08/2023 (Quinta)
05 31 37 47 52 58
Concurso: 2625 - 26/08/2023 (Sábado)
09 10 35 44 55 58
Concurso: 2626 - 29/08/2023 (Terça)
01 09 13 16 52 59
Concurso: 2627 - 31/08/2023 (Quinta)
13 25 31 43 57 58
Setembro/2023
Concurso: 2628 - 02/09/2023 (Sábado)
05 14 32 40 53 54
Concurso: 2629 - 05/09/2023 (Terça)
11 32 35 40 41 48
Concurso: 2630 - 09/09/2023 (Sábado)
14 18 22 26 31 38
Concurso: 2631 - 12/09/2023 (Terça)
14 26 36 39 50 53
Concurso: 2632 - 14/09/2023 (Quinta)
05 10 27 38 56 57
Concurso: 2633 - 16/09/2023 (Sábado)
02 23 25 33 45 54
Concurso: 2634 - 19/09/2023 (Terça)
08 27 28 32 48 56
Concurso: 2635 - 21/09/2023 (Quinta)
06 11 29 37 56 58
Concurso: 2636 - 23/09/2023 (Sábado)
05 16 38 42 43 48
Concurso: 2637 - 26/09/2023 (Terça)
01 02 10 32 34 59
Concurso: 2638 - 28/09/2023 (Quinta)
09 30 34 44 54 55
Concurso: 2639 - 30/09/2023 (Sábado)
02 08 11 22 48 49
Outubro/2023
Concurso: 2640 - 03/10/2023 (Terça)
04 08 10 27 28 32
Concurso: 2641 - 05/10/2023 (Quinta)
09 24 34 39 45 50
Concurso: 2642 - 07/10/2023 (Sábado)
08 13 31 33 49 50
Concurso: 2643 - 10/10/2023 (Terça)
02 10 29 31 56 59
Concurso: 2644 - 14/10/2023 (Sábado)
04 17 22 28 30 49
Concurso: 2645 - 17/10/2023 (Terça)
08 22 34 42 51 59
Concurso: 2646 - 19/10/2023 (Quinta)
18 28 30 39 41 58
Concurso: 2647 - 21/10/2023 (Sábado)
09 33 39 43 50 54
Concurso: 2648 - 24/10/2023 (Terça)
20 44 45 46 56 59
Concurso: 2649 - 26/10/2023 (Quinta)
06 11 26 32 46 56
Concurso: 2650 - 28/10/2023 (Sábado)
09 18 29 37 39 58
Novembro/2023
Concurso: 2651 - 01/11/2023 (Quarta)
06 23 35 36 37 59
Concurso: 2652 - 04/11/2023 (Sábado)
13 23 26 29 45 59
Concurso: 2653 - 07/11/2023 (Terça)
14 32 41 43 48 60
Concurso: 2654 - 09/11/2023 (Quinta)
11 17 23 36 47 51
Concurso: 2655 - 11/11/2023 (Sábado)
10 23 30 31 49 56
Concurso: 2656 - 14/11/2023 (Terça)
20 24 27 46 57 58
Concurso: 2657 - 18/11/2023 (Sábado)
07 27 32 33 36 53
Concurso: 2658 - 21/11/2023 (Terça)
05 13 39 51 58 60
Concurso: 2659 - 23/11/2023 (Quinta)
11 36 46 53 55 60
Concurso: 2660 - 25/11/2023 (Sábado)
06 12 13 20 38 60
Concurso: 2661 - 28/11/2023 (Terça)
06 30 35 38 41 56
Concurso: 2662 - 30/11/2023 (Quinta)
17 20 31 34 40 42
Dezembro/2023
Concurso: 2663 - 02/12/2023 (Sábado)
07 11 27 41 56 59
Concurso: 2664 - 05/12/2023 (Terça)
12 15 17 30 40 52
Concurso: 2665 - 07/12/2023 (Quinta)
03 14 21 22 37 39
Concurso: 2666 - 09/12/2023 (Sábado)
05 25 29 30 43 47
Concurso: 2667 - 12/12/2023 (Terça)
01 04 08 21 46 51
Concurso: 2668 - 14/12/2023 (Quinta)
01 27 30 41 46 57
Concurso: 2669 - 16/12/2023 (Sábado)
04 07 16 35 46 54
Concurso: 2670 - 31/12/2023 (Domingo)
21 24 33 41 48 56
Janeiro/2024
Concurso: 2671 - 04/01/2024 (Quinta)
16 19 43 53 57 58
Concurso: 2672 - 06/01/2024 (Sábado)
10 13 20 40 43 56
Concurso: 2673 - 09/01/2024 (Terça)
04 27 35 45 52 56
Concurso: 2674 - 11/01/2024 (Quinta)
08 14 15 21 23 46
Concurso: 2675 - 13/01/2024 (Sábado)
01 02 03 04 05 06
Concurso: 2676 - 16/01/2024 (Terça)
07 08 09 10 11 12
Concurso: 2677 - 18/01/2024 (Quinta)
13 14 15 16 17 18
`
            },
            quina: {
                numbersPerDraw: 5,
                maxNumber: 80,
                data: `Janeiro/2023

Concurso: 6000 - 02/01/2023 (Segunda)
01 10 20 30 40
Concurso: 6001 - 03/01/2023 (Terça)
02 11 21 31 41
Concurso: 6002 - 04/01/2023 (Quarta)
03 12 22 32 42
Concurso: 6003 - 05/01/2023 (Quinta)
04 13 23 33 43
Concurso: 6004 - 06/01/2023 (Sexta)
05 14 24 34 44
Concurso: 6005 - 07/01/2023 (Sábado)
06 15 25 35 45

Fevereiro/2023

Concurso: 6006 - 01/02/2023 (Quarta)
07 16 26 36 46
Concurso: 6007 - 02/02/2023 (Quinta)
08 17 27 37 47
Concurso: 6008 - 03/02/2023 (Sexta)
09 18 28 38 48
Concurso: 6009 - 04/02/2023 (Sábado)
10 19 29 39 49

Março/2023

Concurso: 6010 - 01/03/2023 (Quarta)
11 20 30 40 50
Concurso: 6011 - 02/03/2023 (Quinta)
12 21 31 41 51
Concurso: 6012 - 03/03/2023 (Sexta)
13 22 32 42 52
Concurso: 6013 - 04/03/2023 (Sábado)
14 23 33 43 53

Abril/2023

Concurso: 6014 - 03/04/2023 (Segunda)
15 24 34 44 54
Concurso: 6015 - 04/04/2023 (Terça)
16 25 35 45 55
Concurso: 6016 - 05/04/2023 (Quarta)
17 26 36 46 56
Concurso: 6017 - 06/04/2023 (Quinta)
18 27 37 47 57
Concurso: 6018 - 07/04/2023 (Sexta)
19 28 38 48 58
Concurso: 6019 - 08/04/2023 (Sábado)
20 29 39 49 59

Maio/2023

Concurso: 6020 - 01/05/2023 (Segunda)
21 30 40 50 60
Concurso: 6021 - 02/05/2023 (Terça)
22 31 41 51 61
Concurso: 6022 - 03/05/2023 (Quarta)
23 32 42 52 62
Concurso: 6023 - 04/05/2023 (Quinta)
24 33 43 53 63
Concurso: 6024 - 05/05/2023 (Sexta)
25 34 44 54 64
Concurso: 6025 - 06/05/2023 (Sábado)
26 35 45 55 65

Junho/2023

Concurso: 6026 - 01/06/2023 (Quinta)
27 36 46 56 66
Concurso: 6027 - 02/06/2023 (Sexta)
28 37 47 57 67
Concurso: 6028 - 03/06/2023 (Sábado)
29 38 48 58 68

Julho/2023

Concurso: 6029 - 03/07/2023 (Segunda)
30 39 49 59 69
Concurso: 6030 - 04/07/2023 (Terça)
31 40 50 60 70
Concurso: 6031 - 05/07/2023 (Quarta)
32 41 51 61 71
Concurso: 6032 - 06/07/2023 (Quinta)
33 42 52 62 72
Concurso: 6033 - 07/07/2023 (Sexta)
34 43 53 63 73
Concurso: 6034 - 08/07/2023 (Sábado)
35 44 54 64 74

Agosto/2023

Concurso: 6035 - 01/08/2023 (Terça)
36 45 55 65 75
Concurso: 6036 - 02/08/2023 (Quarta)
37 46 56 66 76
Concurso: 6037 - 03/08/2023 (Quinta)
38 47 57 67 77
Concurso: 6038 - 04/08/2023 (Sexta)
39 48 58 68 78
Concurso: 6039 - 05/08/2023 (Sábado)
40 49 59 69 79

Setembro/2023

Concurso: 6040 - 01/09/2023 (Sexta)
41 50 60 70 80
Concurso: 6041 - 02/09/2023 (Sábado)
01 02 03 04 05

Outubro/2023

Concurso: 6042 - 02/10/2023 (Segunda)
06 07 08 09 10
Concurso: 6043 - 03/10/2023 (Terça)
11 12 13 14 15
Concurso: 6044 - 04/10/2023 (Quarta)
16 17 18 19 20
Concurso: 6045 - 05/10/2023 (Quinta)
21 22 23 24 25
Concurso: 6046 - 06/10/2023 (Sexta)
26 27 28 29 30
Concurso: 6047 - 07/10/2023 (Sábado)
31 32 33 34 35

Novembro/2023

Concurso: 6048 - 01/11/2023 (Quarta)
36 37 38 39 40
Concurso: 6049 - 02/11/2023 (Quinta)
41 42 43 44 45
Concurso: 6050 - 03/11/2023 (Sexta)
46 47 48 49 50
Concurso: 6051 - 04/11/2023 (Sábado)
51 52 53 54 55

Dezembro/2023

Concurso: 6052 - 01/12/2023 (Sexta)
56 57 58 59 60
Concurso: 6053 - 02/12/2023 (Sábado)
61 62 63 64 65

Janeiro/2024

Concurso: 6300 - 02/01/2024 (Terça)
11 20 30 50 60
Concurso: 6301 - 03/01/2024 (Quarta)
12 21 31 51 61
Concurso: 6302 - 04/01/2024 (Quinta)
13 22 32 52 62

Janeiro/2025

Concurso: 6621 - 02/01/2025 (Quinta)
06 12 42 44 62
Concurso: 6622 - 03/01/2025 (Sexta)
01 02 25 37 79
Concurso: 6623 - 04/01/2025 (Sábado)
27 51 52 54 65
Concurso: 6624 - 06/01/2025 (Segunda)
10 23 48 74 79
Concurso: 6625 - 07/01/2025 (Terça)
17 19 30 48 77
Concurso: 6626 - 08/01/2025 (Quarta)
03 11 19 52 71
Concurso: 6627 - 09/01/2025 (Quinta)
11 13 21 41 54
Concurso: 6628 - 10/01/2025 (Sexta)
33 40 43 48 70
Concurso: 6629 - 11/01/2025 (Sábado)
05 19 26 43 66
Concurso: 6630 - 13/01/2025 (Segunda)
09 37 42 55 71
Concurso: 6631 - 14/01/2025 (Terça)
03 10 57 65 67
Concurso: 6632 - 15/01/2025 (Quarta)
15 24 45 52 70
Concurso: 6633 - 16/01/2025 (Quinta)
25 41 59 75 77
Concurso: 6634 - 17/01/2025 (Sexta)
23 32 37 71 72
Concurso: 6635 - 18/01/2025 (Sábado)
13 38 49 63 66
Concurso: 6636 - 20/01/2025 (Segunda)
02 22 31 35 53
Concurso: 6637 - 21/01/2025 (Terça)
13 33 34 46 70
Concurso: 6638 - 22/01/2025 (Quarta)
13 26 40 57 75
Concurso: 6639 - 23/01/2025 (Quinta)
19 26 72 76 77
Concurso: 6640 - 24/01/2025 (Sexta)
01 05 08 77 78
Concurso: 6641 - 25/01/2025 (Sábado)
22 52 60 63 78
Concurso: 6642 - 27/01/2025 (Segunda)
18 21 27 57 72
Concurso: 6643 - 28/01/2025 (Terça)
08 28 42 47 78
Concurso: 6644 - 29/01/2025 (Quarta)
08 18 59 60 75
Concurso: 6645 - 30/01/2025 (Quinta)
05 19 48 64 76
Concurso: 6646 - 31/01/2025 (Sexta)
07 41 65 77 79

Fevereiro/2025

Concurso: 6647 - 01/02/2025 (Sábado)
05 14 31 43 51
Concurso: 6648 - 03/02/2025 (Segunda)
16 25 39 58 68
Concurso: 6649 - 04/02/2025 (Terça)
09 43 47 65 74
Concurso: 6650 - 05/02/2025 (Quarta)
16 23 24 27 48
Concurso: 6651 - 06/02/2025 (Quinta)
14 23 31 48 63
Concurso: 6652 - 07/02/2025 (Sexta)
42 47 52 59 65
Concurso: 6653 - 08/02/2025 (Sábado)
13 17 40 49 51
Concurso: 6654 - 10/02/2025 (Segunda)
14 25 48 51 66
Concurso: 6655 - 11/02/2025 (Terça)
10 34 41 44 69
Concurso: 6656 - 12/02/2025 (Quarta)
17 24 31 66 79
Concurso: 6657 - 13/02/2025 (Quinta)
10 30 41 53 69
Concurso: 6658 - 14/02/2025 (Sexta)
15 22 56 60 71
Concurso: 6659 - 15/02/2025 (Sábado)
05 12 20 69 70
Concurso: 6660 - 17/02/2025 (Segunda)
05 15 21 35 43
Concurso: 6661 - 18/02/2025 (Terça)
28 45 48 72 76
Concurso: 6662 - 19/02/2025 (Quarta)
04 34 40 61 66
Concurso: 6663 - 20/02/2025 (Quinta)
21 35 44 52 62
Concurso: 6664 - 21/02/2025 (Sexta)
11 23 32 44 76
Concurso: 6665 - 22/02/2025 (Sábado)
26 30 35 66 77
Concurso: 6666 - 24/02/2025 (Segunda)
10 20 23 25 74
Concurso: 6667 - 25/02/2025 (Terça)
02 03 19 42 48
Concurso: 6668 - 26/02/2025 (Quarta)
19 31 32 54 78
Concurso: 6669 - 27/02/2025 (Quinta)
01 31 33 60 69
Concurso: 6670 - 28/02/2025 (Sexta)
05 14 21 31 65

Março/2025

Concurso: 6671 - 01/03/2025 (Sábado)
46 47 61 66 75
Concurso: 6672 - 05/03/2025 (Quarta)
27 30 48 66 73
Concurso: 6673 - 06/03/2025 (Quinta)
14 26 35 52 74
Concurso: 6674 - 07/03/2025 (Sexta)
19 32 57 60 75
Concurso: 6675 - 08/03/2025 (Sábado)
13 25 26 37 76
Concurso: 6676 - 10/03/2025 (Segunda)
11 25 29 31 45
Concurso: 6677 - 11/03/2025 (Terça)
17 52 69 72 75
Concurso: 6678 - 12/03/2025 (Quarta)
33 36 50 51 69
Concurso: 6679 - 13/03/2025 (Quinta)
03 05 16 28 37
Concurso: 6680 - 14/03/2025 (Sexta)
08 13 22 34 41
Concurso: 6681 - 15/03/2025 (Sábado)
39 46 52 65 77
Concurso: 6682 - 17/03/2025 (Segunda)
08 15 24 28 68
Concurso: 6683 - 18/03/2025 (Terça)
20 23 53 60 71
Concurso: 6684 - 19/03/2025 (Quarta)
09 15 27 65 76
Concurso: 6685 - 20/03/2025 (Quinta)
06 20 37 51 69
Concurso: 6686 - 21/03/2025 (Sexta)
15 16 25 35 67
Concurso: 6687 - 22/03/2025 (Sábado)
10 11 39 41 50
Concurso: 6688 - 24/03/2025 (Segunda)
21 40 43 67 68
Concurso: 6689 - 25/03/2025 (Terça)
07 23 24 55 56
Concurso: 6690 - 26/03/2025 (Quarta)
01 33 53 55 76
Concurso: 6691 - 27/03/2025 (Quinta)
06 07 38 56 67
Concurso: 6692 - 28/03/2025 (Sexta)
07 13 21 34 76
Concurso: 6693 - 29/03/2025 (Sábado)
13 17 20 29 46
Concurso: 6694 - 31/03/2025 (Segunda)
24 38 43 44 49

Abril/2025

Concurso: 6695 - 01/04/2025 (Terça)
22 62 67 70 79
Concurso: 6696 - 02/04/2025 (Quarta)
15 19 51 55 57
Concurso: 6697 - 03/04/2025 (Quinta)
40 42 44 47 52
Concurso: 6698 - 04/04/2025 (Sexta)
07 13 18 22 32
Concurso: 6699 - 05/04/2025 (Sábado)
08 13 41 48 78
Concurso: 6700 - 07/04/2025 (Segunda)
12 21 23 31 42
Concurso: 6701 - 08/04/2025 (Terça)
04 09 21 29 64
Concurso: 6702 - 09/04/2025 (Quarta)
27 43 51 53 78
Concurso: 6703 - 10/04/2025 (Quinta)
08 16 53 61 74
Concurso: 6704 - 11/04/2025 (Sexta)
09 23 26 44 70
Concurso: 6705 - 12/04/2025 (Sábado)
19 27 34 50 72
Concurso: 6706 - 14/04/2025 (Segunda)
40 41 65 66 71
Concurso: 6707 - 15/04/2025 (Terça)
14 32 58 70 77
Concurso: 6708 - 16/04/2025 (Quarta)
09 13 15 40 77
Concurso: 6709 - 17/04/2025 (Quinta)
29 32 39 57 61
Concurso: 6710 - 19/04/2025 (Sábado)
13 17 18 29 78
Concurso: 6711 - 22/04/2025 (Terça)
10 15 48 52 71
Concurso: 6712 - 23/04/2025 (Quarta)
01 27 49 57 63
Concurso: 6713 - 24/04/2025 (Quinta)
35 43 48 52 72
Concurso: 6714 - 25/04/2025 (Sexta)
20 31 47 59 72
Concurso: 6715 - 26/04/2025 (Sábado)
09 14 28 37 51
Concurso: 6716 - 28/04/2025 (Segunda)
18 22 60 61 62
Concurso: 6717 - 29/04/2025 (Terça)
06 13 62 72 77
Concurso: 6718 - 30/04/2025 (Quarta)
19 36 53 65 66

Maio/2025

Concurso: 6719 - 02/05/2025 (Sexta)
07 09 15 37 44
Concurso: 6720 - 03/05/2025 (Sábado)
04 42 55 61 68
Concurso: 6721 - 05/05/2025 (Segunda)
05 43 56 74 79
Concurso: 6722 - 06/05/2025 (Terça)
07 15 24 32 52
Concurso: 6723 - 07/05/2025 (Quarta)
15 41 51 56 79
Concurso: 6724 - 08/05/2025 (Quinta)
12 13 22 54 74
Concurso: 6725 - 09/05/2025 (Sexta)
20 23 30 39 53
Concurso: 6726 - 10/05/2025 (Sábado)
20 49 56 60 67
Concurso: 6727 - 12/05/2025 (Segunda)
02 06 14 16 60
Concurso: 6728 - 13/05/2025 (Terça)
08 15 38 44 80
Concurso: 6729 - 14/05/2025 (Quarta)
17 20 25 37 39
Concurso: 6730 - 15/05/2025 (Quinta)
21 44 57 60 65
Concurso: 6731 - 16/05/2025 (Sexta)
24 41 51 57 62
Concurso: 6732 - 17/05/2025 (Sábado)
13 19 22 43 64
Concurso: 6733 - 19/05/2025 (Segunda)
18 34 36 61 70
Concurso: 6734 - 20/05/2025 (Terça)
17 32 44 65 80
Concurso: 6735 - 21/05/2025 (Quarta)
30 49 50 73 79
Concurso: 6736 - 22/05/2025 (Quinta)
17 29 60 75 80
Concurso: 6737 - 23/05/2025 (Sexta)
03 43 63 67 74
Concurso: 6738 - 24/05/2025 (Sábado)
02 47 60 61 62
Concurso: 6739 - 26/05/2025 (Segunda)
10 31 48 74 79
Concurso: 6740 - 27/05/2025 (Terça)
21 39 46 58 71
`
            }
        };

        let currentLottery = 'megaSena';
        let allDraws = [];
        let filteredDraws = [];
        let numberFrequencies = {};

        // Get DOM elements
        const lotterySelect = document.getElementById('lotterySelect');
        const pastDrawsTextarea = document.getElementById('pastDraws');
        const errorMessageDiv = document.getElementById('errorMessage');
        const filterMonthSelect = document.getElementById('filterMonth');
        const filterYearSelect = document.getElementById('filterYear');
        const noDrawsMessage = document.getElementById('noDrawsMessage');
        const frequencyAnalysisSection = document.getElementById('frequencyAnalysisSection');
        const mostCommonNumbersDiv = document.getElementById('mostCommonNumbers');
        const frequencyDisclaimer = document.getElementById('frequencyDisclaimer');
        // Removed filteredDrawsSection and filteredDrawsListDiv as per request
        const numGamesInput = document.getElementById('numGames');
        const strategySelect = document.getElementById('strategy');
        const generateGamesButton = document.getElementById('generateGamesButton');
        const generatedGamesSection = document.getElementById('generatedGamesSection');
        const generatedGamesListDiv = document.getElementById('generatedGamesList');

        /**
         * Parses the raw lottery draw data into an array of draw objects.
         * Each draw object contains year, month, and an array of numbers.
         * @param {string} rawData - The raw text data of past draws.
         * @param {number} numbersPerDraw - The expected number of numbers per draw.
         * @returns {Array<Object>} An array of parsed draw objects.
         */
        function parseDraws(rawData, numbersPerDraw) {
            const draws = [];
            const lines = rawData.split('\n').map(line => line.trim()).filter(line => line.length > 0);
            let currentYear = '';
            let currentMonth = '';

            for (let i = 0; i < lines.length; i++) {
                const line = lines[i];

                // Check for month/year headers (e.g., "Janeiro/2015" or "Quina - Janeiro/2023")
                const monthYearMatch = line.match(/(?:Mega-Sena|Quina)?\s*([a-zA-ZçÇãÃõÕéÉíÍóÓúÚ]+)\/(\d{4})/);
                if (monthYearMatch) {
                    currentMonth = monthYearMatch[1];
                    currentYear = monthYearMatch[2];
                    continue;
                }

                // Check for draw lines (e.g., "Concurso: 1666 - 03/01/2015 (Sábado)")
                const drawInfoMatch = line.match(/Concurso:\s*(\d+)\s*-\s*(\d{2}\/\d{2}\/(\d{4}))\s*\(([^)]+)\)/);
                if (drawInfoMatch) {
                    // The next line should contain the numbers
                    if (i + 1 < lines.length) {
                        const numbersLine = lines[i + 1];
                        const numbers = numbersLine.split(' ').map(Number).filter(n => !isNaN(n) && n > 0);

                        if (numbers.length === numbersPerDraw) {
                            const monthNum = drawInfoMatch[2].substring(3, 5); // Extract MM from DD/MM/YYYY
                            const drawMonth = monthNumToName(monthNum);
                            const drawYear = drawInfoMatch[3];

                            draws.push({
                                year: drawYear,
                                month: drawMonth,
                                numbers: numbers.sort((a, b) => a - b) // Sort numbers for consistency
                            });
                            i++; // Skip the numbers line as it's already processed
                        } else if (numbers.length > 0) {
                            console.warn(`Skipping malformed line (unexpected number count for ${currentLottery}): ${numbersLine}`);
                        }
                    }
                }
            }
            return draws;
        }


        /**
         * Converts a numeric month string to its Portuguese name.
         * @param {string} monthNum - The numeric month (e.g., "01").
         * @returns {string} The Portuguese month name (e.g., "Janeiro").
         */
        function monthNumToName(monthNum) {
            const months = {
                '01': 'Janeiro', '02': 'Fevereiro', '03': 'Março', '04': 'Abril',
                '05': 'Maio', '06': 'Junho', '07': 'Julho', '08': 'Agosto',
                '09': 'Setembro', '10': 'Outubro', '11': 'Novembro', '12': 'Dezembro'
            };
            return months[monthNum] || '';
        }

        /**
         * Calculates the frequency of each number in the given draws.
         * @param {Array<Object>} draws - An array of draw objects.
         * @param {number} maxNumber - The maximum possible number in the lottery.
         * @returns {Object} An object where keys are numbers and values are their frequencies.
         */
        function calculateFrequency(draws, maxNumber) {
            const frequency = {};
            for (let i = 1; i <= maxNumber; i++) {
                frequency[i] = 0;
            }
            draws.forEach(draw => {
                draw.numbers.forEach(num => {
                    if (num >= 1 && num <= maxNumber) {
                        frequency[num]++;
                    }
                });
            });
            return frequency;
        }

        /**
         * Renders the most frequent numbers in the frequency analysis section.
         * @param {Object} frequency - The frequency map of numbers.
         * @param {number} numbersToPick - The number of numbers required for a draw.
         */
        function renderFrequency(frequency, numbersToPick) {
            const sortedFrequencies = Object.entries(frequency)
                .sort(([, countA], [, countB]) => countB - countA)
                .map(([num, count]) => ({ number: Number(num), count }));

            mostCommonNumbersDiv.innerHTML = ''; // Clear previous content

            // Display top 20 most frequent numbers
            sortedFrequencies.slice(0, 20).forEach(item => {
                const span = document.createElement('span');
                span.className = 'bg-gray-200 text-gray-800 font-semibold py-1 px-3 rounded-full text-sm flex items-center justify-center';
                span.textContent = `${String(item.number).padStart(2, '0')} (${item.count})`;
                mostCommonNumbersDiv.appendChild(span);
            });

            // Show section and disclaimer if there are frequencies
            if (sortedFrequencies.length > 0) {
                frequencyAnalysisSection.classList.remove('hidden');
                frequencyDisclaimer.classList.remove('hidden');
            } else {
                frequencyAnalysisSection.classList.add('hidden');
                frequencyDisclaimer.classList.add('hidden');
            }
        }

        /**
         * Filters the draws based on the selected month and year.
         * @param {Array<Object>} draws - All parsed draw objects.
         * @param {string} monthFilter - The selected month (e.g., "Janeiro" or "all").
         * @param {string} yearFilter - The selected year (e.g., "2023" or "all").
         * @returns {Array<Object>} An array of filtered draw objects.
         */
        function filterDraws(draws, monthFilter, yearFilter) {
            return draws.filter(draw => {
                const matchesMonth = monthFilter === 'all' || draw.month === monthFilter;
                const matchesYear = yearFilter === 'all' || draw.year === yearFilter;
                return matchesMonth && matchesYear;
            });
        }

        /**
         * Updates the year filter options based on the available data.
         * @param {Array<Object>} draws - All parsed draw objects.
         */
        function updateYearFilterOptions(draws) {
            const years = new Set(draws.map(draw => draw.year));
            filterYearSelect.innerHTML = '<option value="all">Todos os Anos</option>';
            Array.from(years).sort().forEach(year => {
                const option = document.createElement('option');
                option.value = year;
                option.textContent = year;
                filterYearSelect.appendChild(option);
            });
        }

        /**
         * Generates a single set of lottery numbers based on the chosen strategy.
         * @param {Object} frequency - The frequency map of numbers.
         * @param {number} numbersToPick - The number of numbers to pick for a draw.
         * @param {number} maxNumber - The maximum possible number in the lottery.
         * @param {string} strategy - The generation strategy ('frequencia' or 'ponderada').
         * @returns {Array<number>} An array of generated numbers.
         */
        function generateNumbers(frequency, numbersToPick, maxNumber, strategy) {
            const generated = new Set();
            const allPossibleNumbers = Array.from({ length: maxNumber }, (_, i) => i + 1);

            if (strategy === 'frequencia') {
                // Direct frequency: pick the most frequent numbers
                const sortedFrequencies = Object.entries(frequency)
                    .sort(([, countA], [, countB]) => countB - countA)
                    .map(([num]) => Number(num));

                for (let i = 0; generated.size < numbersToPick && i < sortedFrequencies.length; i++) {
                    generated.add(sortedFrequencies[i]);
                }
            } else if (strategy === 'ponderada') {
                // Weighted random: numbers with higher frequency have higher chance
                const weightedNumbers = [];
                for (const num in frequency) {
                    for (let i = 0; i < frequency[num]; i++) {
                        weightedNumbers.push(Number(num));
                    }
                }

                if (weightedNumbers.length === 0) {
                    // Fallback to pure random if no frequency data
                    while (generated.size < numbersToPick) {
                        const randomNumber = Math.floor(Math.random() * maxNumber) + 1;
                        generated.add(randomNumber);
                    }
                } else {
                    while (generated.size < numbersToPick) {
                        const randomIndex = Math.floor(Math.random() * weightedNumbers.length);
                        generated.add(weightedNumbers[randomIndex]);
                    }
                }
            }

            // If for some reason we don't have enough numbers (e.g., not enough unique frequent numbers),
            // fill with random unique numbers.
            while (generated.size < numbersToPick) {
                const randomNumber = Math.floor(Math.random() * maxNumber) + 1;
                generated.add(randomNumber);
            }

            return Array.from(generated).sort((a, b) => a - b);
        }

        /**
         * Handles the change in lottery selection.
         */
        function handleLotteryChange() {
            currentLottery = lotterySelect.value;
            const data = lotteryData[currentLottery].data;
            const numbersPerDraw = lotteryData[currentLottery].numbersPerDraw;
            const maxNumber = lotteryData[currentLottery].maxNumber;

            pastDrawsTextarea.value = data;
            errorMessageDiv.textContent = '';
            generatedGamesListDiv.innerHTML = '';
            generatedGamesSection.classList.add('hidden');

            try {
                allDraws = parseDraws(data, numbersPerDraw);
                updateYearFilterOptions(allDraws);
                applyFiltersAndRecalculate();
            } catch (error) {
                errorMessageDiv.textContent = `Erro ao processar dados: ${error.message}`;
                allDraws = [];
                filteredDraws = [];
                numberFrequencies = {};
                renderFrequency({}, numbersPerDraw);
            }
        }

        /**
         * Applies the current filters and recalculates frequencies and renders data.
         */
        function applyFiltersAndRecalculate() {
            const selectedMonth = filterMonthSelect.value === 'all' ? 'all' : filterMonthSelect.options[filterMonthSelect.selectedIndex].textContent;
            const selectedYear = filterYearSelect.value;

            filteredDraws = filterDraws(allDraws, selectedMonth, selectedYear);
            // Removed call to renderFilteredDraws(filteredDraws);

            const numbersPerDraw = lotteryData[currentLottery].numbersPerDraw;
            const maxNumber = lotteryData[currentLottery].maxNumber;
            numberFrequencies = calculateFrequency(filteredDraws, maxNumber);
            renderFrequency(numberFrequencies, numbersPerDraw);

            if (filteredDraws.length === 0) {
                noDrawsMessage.classList.remove('hidden');
            } else {
                noDrawsMessage.classList.add('hidden');
            }
        }

        // Event Listeners
        lotterySelect.addEventListener('change', handleLotteryChange);
        filterMonthSelect.addEventListener('change', applyFiltersAndRecalculate);
        filterYearSelect.addEventListener('change', applyFiltersAndRecalculate);

        generateGamesButton.addEventListener('click', () => {
            const numGamesToGenerate = parseInt(numGamesInput.value, 10);
            const selectedStrategy = strategySelect.value;
            const numbersPerDraw = lotteryData[currentLottery].numbersPerDraw;
            const maxNumber = lotteryData[currentLottery].maxNumber;

            if (filteredDraws.length === 0) {
                generatedGamesListDiv.innerHTML = `<p class="text-red-600">Não há dados de sorteios filtrados para gerar jogos.</p>`;
                generatedGamesSection.classList.remove('hidden');
                return;
            }

            generatedGamesListDiv.innerHTML = ''; // Clear previous games
            generatedGamesSection.classList.remove('hidden');

            for (let i = 0; i < numGamesToGenerate; i++) {
                const game = generateNumbers(numberFrequencies, numbersPerDraw, maxNumber, selectedStrategy);
                const gameDiv = document.createElement('div');
                gameDiv.className = 'bg-white p-4 rounded-lg shadow-md border border-green-100 flex flex-wrap items-center justify-center gap-3';
                gameDiv.innerHTML = `<span class="text-gray-700 font-semibold mr-2">Jogo ${i + 1}:</span>`;
                game.forEach(num => {
                    const numSpan = document.createElement('span');
                    numSpan.className = 'bg-green-500 text-white font-bold text-lg w-10 h-10 flex items-center justify-center rounded-full shadow-md';
                    numSpan.textContent = String(num).padStart(2, '0');
                    gameDiv.appendChild(numSpan);
                });
                generatedGamesListDiv.appendChild(gameDiv);
            }
        });

        // Initial load
        window.onload = handleLotteryChange; // Call on page load to initialize with Mega-Sena data
    </script>
</body>
</html>
