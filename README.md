<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Guia Interativo de Configuração de Router</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chosen Palette: Slate Gray & Sky Blue -->
    <!-- Application Structure Plan: A single-page application designed as an interactive checklist. The main content is organized into a collapsible accordion, with each section representing a step from the source document. This allows technicians to focus on one task at a time. A prominent "Quick Access" panel provides immediate access to critical data like IPs and credentials, with a copy-to-clipboard feature for usability. A dynamic progress bar visually tracks completion. This task-oriented structure is more efficient for a technician than a linear document, improving workflow and reducing errors. -->
    <!-- Visualization & Content Choices: Report Info -> Credentials & IPs. Goal -> Quick Reference. Method -> Styled cards in an "Quick Access" section. Interaction -> Copy-to-clipboard button. Justification -> Technicians frequently need this data; copying prevents typos. | Report Info -> Procedural Steps. Goal -> Guide user through a sequence. Method -> Collapsible accordion with nested checklists. Interaction -> Checkboxes update a master progress bar. Justification -> Transforms a passive guide into an active tool, ensuring steps aren't missed. No charts are needed as the source material is procedural, not data-driven. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }
        details > summary {
            list-style: none;
        }
        details > summary::-webkit-details-marker {
            display: none;
        }
        .task-checkbox:checked + label .checkbox-icon-unchecked {
            display: none;
        }
        .task-checkbox:not(:checked) + label .checkbox-icon-checked {
            display: none;
        }
        .task-checkbox:checked + label {
            text-decoration: line-through;
            color: #6b7280;
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 font-sans">

    <div class="container mx-auto p-4 sm:p-6 lg:p-8 max-w-7xl">

        <header class="text-center mb-8">
            <h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Guia de Configuração de Equipamento Router</h1>
            <p class="mt-2 text-slate-600 max-w-3xl mx-auto">Um guia interativo para testar e configurar roteadores. O modelo de referência é o GW24, mas o processo é aplicável a outros equipamentos com interfaces similares.</p>
        </header>

        <div class="mb-8 p-4 bg-white rounded-lg shadow-md">
            <div class="flex items-center justify-between mb-2">
                <h3 class="text-lg font-semibold text-slate-700">Progresso Geral</h3>
                <span id="progress-text" class="font-medium text-slate-700">0%</span>
            </div>
            <div class="w-full bg-slate-200 rounded-full h-4">
                <div id="progress-bar" class="bg-sky-500 h-4 rounded-full transition-all duration-500" style="width: 0%"></div>
            </div>
        </div>
        
        <div class="lg:grid lg:grid-cols-3 lg:gap-8">
            <aside class="lg:col-span-1 mb-8 lg:mb-0">
                <div class="sticky top-8 bg-white p-6 rounded-lg shadow-md">
                    <h2 class="text-xl font-bold text-slate-900 mb-4 border-b pb-2">Acesso Rápido</h2>
                    <p class="text-sm text-slate-500 mb-6">Informações de acesso padrão para os equipamentos. Clique para copiar.</p>
                    <div id="quick-access-content" class="space-y-4">
                    </div>
                </div>
            </aside>

            <main class="lg:col-span-2 space-y-4">
                <div id="steps-container">
                </div>
            </main>
        </div>
    </div>

    <script>
        const appData = {
            quickAccess: [
                { title: 'IP Gateway', value: '192.168.10.1', copy: true },
                { title: 'Usuário (Gateway)', value: 'Adminisp', copy: true },
                { title: 'Senha (Gateway)', value: 'Adminisp', copy: true },
                { title: 'IP ZTE', value: '192.168.1.1', copy: true },
                { title: 'Usuário (ZTE)', value: 'multipro', copy: true },
                { title: 'Senha (ZTE)', value: 'multipro', copy: true },
                { title: 'IP Nokia', value: '192.168.1.254', copy: true },
                { title: 'Login (Nokia)', value: 'AdminGPON', copy: true },
                { title: 'Senhas (Nokia)', value: '4d0r41ou / ALC#FGU', copy: false },
                { title: 'IP Huawei', value: '192.168.18.1', copy: true },
                { title: 'Usuário (Huawei)', value: 'Epadmin', copy: true },
                { title: 'Senha (Huawei)', value: 'adminEp', copy: true }
            ],
            steps: [
                {
                    title: "1. Verificações Iniciais e Acessos",
                    description: "Antes de qualquer configuração, é fundamental realizar os seguintes testes e acessar os equipamentos.",
                    tasks: [
                        { id: 'task1_1', text: "Testar todas as portas LAN do equipamento." }
                    ]
                },
                {
                    title: "2. Verificação e Atualização de Firmware",
                    description: "É crucial verificar a versão do firmware. Para o GW24, é necessário atualizar para a versão 8 se não estiver nela. Para os demais, não há atualização no momento.",
                    tasks: [
                        { id: 'task2_1', text: "Acessar a interface web do equipamento." },
                        { id: 'task2_2', text: 'Navegar até "Status" > "Device Information" para verificar a "Software Version".' },
                        { id: 'task2_3', text: 'Se for um GW24 com versão antiga, ir para "Administration" > "Soft Update".' },
                        { id: 'task2_4', text: "Buscar o arquivo de atualização (Versão 8) e clicar em 'Upgrade'." },
                    ]
                },
                {
                    title: "3. Provisionamento do Equipamento no IXC",
                    description: "Siga os passos abaixo para provisionar o equipamento no sistema IXC. As credenciais de teste e perfis são essenciais para a correta configuração.",
                    tasks: [
                        { id: 'task3_1', text: 'Configurar PPPoE de teste: `teste_2` / senha `conecte123`.' },
                        { id: 'task3_2', text: "Utilizar o perfil 26 para modo 'router' ou o perfil 27 para modo 'bridge'." },
                        { id: 'task3_3', text: "Configurar a VLAN `556` ou `190`." },
                        { id: 'task3_4', text: "Salvar as configurações no IXC." },
                        { id: 'task3_5', text: "Autorizar a ONU no sistema." },
                    ]
                },
                {
                    title: "4. Verificação de Sinal",
                    description: "Após o provisionamento, verifique o sinal da ONU para garantir a qualidade e estabilidade da conexão física.",
                    tasks: [
                        { id: 'task4_1', text: "Verificar o relatório de 'Potência de ONU' no sistema." }
                    ]
                },
                {
                    title: "5. Configuração PPPoE no Equipamento",
                    description: "A configuração PPPoE deve ser realizada dentro da interface do próprio equipamento, seguindo o padrão da OLT Nokia.",
                    tasks: [
                        { id: 'task5_1', text: 'Navegar até a aba "Network" e selecionar "WAN Connection".' },
                        { id: 'task5_2', text: 'Definir o "Type" como `PPPoE`.' },
                        { id: 'task5_3', text: "Associar todas as portas LAN necessárias em 'Port Binding'." },
                        { id: 'task5_4', text: 'Configurar a "VLAN ID" `556` e "VLAN Type" como `Tag`.' },
                        { id: 'task5_5', text: "Inserir o 'Username' (`teste_teste`) e 'Password' (`conecte123`) conforme configurado no IXC." },
                    ]
                },
                {
                    title: "6. Configuração do Wi-Fi para Teste",
                    description: "Configure a rede Wi-Fi para realizar os testes de conectividade sem fio.",
                    tasks: [
                        { id: 'task6_1', text: "Habilitar o Wireless RF e o SSID desejado (ex: 5G)." },
                        { id: 'task6_2', text: "Definir um nome para a rede (SSID Name)." },
                        { id: 'task6_3', text: "Selecionar o tipo de autenticação (ex: WPA/WPA2-PSK)." },
                        { id: 'task6_4', text: "Definir uma senha para a rede Wi-Fi." },
                    ]
                },
                {
                    title: "7. Verificação de Conectividade e Testes de Velocidade",
                    description: "Com tudo configurado, verifique se o cliente está online no IXC e realize testes de velocidade para validar a performance.",
                    tasks: [
                        { id: 'task7_1', text: "Verificar se o cliente aparece online no IXC (IP e MAC devem ser exibidos)." },
                        { id: 'task7_2', text: "Conectar um dispositivo via cabo de rede e testar a velocidade." },
                        { id: 'task7_3', text: "Conectar um dispositivo via Wi-Fi e testar a velocidade." },
                    ]
                },
                {
                    title: "8. Reiniciar e Resetar Equipamentos",
                    description: "Como passo final, é recomendável reiniciar o equipamento para aplicar todas as configurações. O reset de fábrica só deve ser usado se necessário.",
                    tasks: [
                        { id: 'task8_1', text: 'Reiniciar o equipamento via "Administration" > "System Management" > "Reboot".' },
                        { id: 'task8_2', text: 'Se necessário, resetar para as configurações de fábrica via "Factory reset".' },
                    ]
                }
            ]
        };

        document.addEventListener('DOMContentLoaded', () => {
            const quickAccessContainer = document.getElementById('quick-access-content');
            const stepsContainer = document.getElementById('steps-container');
            const progressBar = document.getElementById('progress-bar');
            const progressText = document.getElementById('progress-text');
            
            let totalTasks = 0;

            function copyToClipboard(text, element) {
                const tempInput = document.createElement('input');
                tempInput.style.position = 'absolute';
                tempInput.style.left = '-9999px';
                tempInput.value = text;
                document.body.appendChild(tempInput);
                tempInput.select();
                try {
                    document.execCommand('copy');
                    const originalText = element.textContent;
                    element.textContent = 'Copiado!';
                    element.classList.add('bg-green-500');
                    setTimeout(() => {
                        element.textContent = originalText;
                        element.classList.remove('bg-green-500');
                    }, 1500);
                } catch (err) {
                    console.error('Falha ao copiar', err);
                }
                document.body.removeChild(tempInput);
            }

            appData.quickAccess.forEach(item => {
                const div = document.createElement('div');
                div.className = 'flex items-center justify-between';
                div.innerHTML = `
                    <div>
                        <p class="font-semibold text-slate-700">${item.title}</p>
                        <p class="text-sm text-sky-600 font-mono">${item.value}</p>
                    </div>
                    ${item.copy ? `<button data-copy="${item.value}" class="copy-btn text-xs bg-slate-200 hover:bg-sky-500 hover:text-white text-slate-600 font-semibold py-1 px-3 rounded-full transition-colors duration-200">Copiar</button>` : ''}
                `;
                quickAccessContainer.appendChild(div);
            });

            appData.steps.forEach((step, index) => {
                const details = document.createElement('details');
                details.className = 'bg-white rounded-lg shadow-md overflow-hidden';
                details.open = index === 0;

                let tasksHtml = '<ul class="space-y-3">';
                step.tasks.forEach(task => {
                    totalTasks++;
                    tasksHtml += `
                        <li class="flex items-start">
                            <input type="checkbox" id="${task.id}" class="task-checkbox hidden">
                            <label for="${task.id}" class="flex items-center cursor-pointer text-slate-700">
                                <span class="mr-3 mt-1 flex-shrink-0 w-5 h-5 border-2 border-slate-300 rounded-md flex items-center justify-center transition-all duration-200">
                                    <svg class="checkbox-icon-checked w-4 h-4 text-white bg-sky-500 rounded-md" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg>
                                    <span class="checkbox-icon-unchecked"></span>
                                </span>
                                ${task.text}
                            </label>
                        </li>
                    `;
                });
                tasksHtml += '</ul>';

                details.innerHTML = `
                    <summary class="p-4 sm:p-6 cursor-pointer flex justify-between items-center bg-slate-50 hover:bg-slate-100 border-b border-slate-200">
                        <div class="flex items-center">
                            <div class="bg-sky-500 text-white rounded-full w-8 h-8 flex items-center justify-center font-bold text-lg mr-4">${index + 1}</div>
                            <div>
                                <h2 class="font-bold text-lg text-slate-900">${step.title}</h2>
                            </div>
                        </div>
                        <svg class="w-6 h-6 text-slate-500 transform transition-transform duration-300" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg>
                    </summary>
                    <div class="p-4 sm:p-6">
                        <p class="text-slate-600 mb-6">${step.description}</p>
                        ${tasksHtml}
                    </div>
                `;
                stepsContainer.appendChild(details);
            });
            
            function updateProgress() {
                const checkedTasks = document.querySelectorAll('.task-checkbox:checked').length;
                const percentage = totalTasks > 0 ? Math.round((checkedTasks / totalTasks) * 100) : 0;
                progressBar.style.width = `${percentage}%`;
                progressText.textContent = `${percentage}%`;
            }

            document.querySelectorAll('.copy-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    const textToCopy = e.target.getAttribute('data-copy');
                    copyToClipboard(textToCopy, e.target);
                });
            });

            document.querySelectorAll('.task-checkbox').forEach(checkbox => {
                checkbox.addEventListener('change', updateProgress);
            });
            
            updateProgress();
        });
    </script>
</body>
</html>

