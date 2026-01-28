<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Agendamento Grupo RODLIDER</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&display=swap');
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: #020617;
            color: #f8fafc;
        }

        .gold-gradient {
            background: linear-gradient(to right, #bf953f, #fcf6ba, #b38728, #fcf6ba, #aa771c);
        }

        .gold-text {
            background: linear-gradient(to right, #bf953f, #fcf6ba, #aa771c);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .gold-border {
            border-color: rgba(191, 149, 63, 0.3);
        }

        .premium-card {
            background: rgba(15, 23, 42, 0.6);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(191, 149, 63, 0.2);
        }

        .time-slot:hover:not(.selected) {
            border-color: rgba(191, 149, 63, 0.5);
            color: #fcf6ba;
        }

        .selected {
            background: linear-gradient(to right, #bf953f, #fcf6ba, #aa771c);
            color: #020617 !important;
            font-weight: 900;
            transform: scale(1.05);
            border-color: transparent;
        }

        /* Animações simples */
        .fade-in { animation: fadeIn 0.5s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body class="min-h-screen p-4 md:p-8">

    <div class="max-w-md mx-auto">
        <!-- Header -->
        <header class="flex flex-col items-center mb-12 text-center fade-in">
            <div class="w-16 h-16 gold-gradient rounded-2xl flex items-center justify-center shadow-2xl mb-4 transform rotate-3">
                <i data-lucide="building-2" class="text-slate-950 w-8 h-8"></i>
            </div>
            <div>
                <h1 class="text-2xl font-black tracking-tighter uppercase leading-none gold-text">AGENDAMENTO</h1>
                <p class="text-amber-200/30 font-black text-[10px] uppercase tracking-[0.6em] mt-1">Grupo RODLIDER</p>
            </div>
        </header>

        <!-- Main Content -->
        <main id="app-content">
            <!-- Passo 1: Seleção -->
            <div id="step-1" class="space-y-8 fade-in">
                <!-- Seletor de Data -->
                <div class="premium-card p-6 rounded-[2rem] shadow-2xl">
                    <div class="flex items-center justify-between">
                        <button onclick="changeDate(-1)" id="prev-btn" class="p-2 text-amber-200/40 hover:text-amber-200 disabled:opacity-5 transition-colors">
                            <i data-lucide="chevron-left" class="w-8 h-8"></i>
                        </button>
                        <div class="text-center">
                            <h2 id="current-date-display" class="text-xl font-black tracking-tight gold-text">--/--/----</h2>
                            <p class="text-[10px] text-amber-100/30 font-bold uppercase tracking-[0.4em] mt-1">Selecione o Dia</p>
                        </div>
                        <button onclick="changeDate(1)" class="p-2 text-amber-200/40 hover:text-amber-200 transition-colors">
                            <i data-lucide="chevron-right" class="w-8 h-8"></i>
                        </button>
                    </div>
                </div>

                <div id="time-grid-container" class="space-y-8">
                    <!-- Gerado via JS -->
                </div>

                <button id="next-step-btn" onclick="goToStep(2)" class="hidden w-full gold-gradient text-slate-950 py-5 rounded-2xl font-black text-xs uppercase tracking-[0.2em] shadow-2xl hover:brightness-110 active:scale-95 transition-all flex items-center justify-center">
                    Continuar para Dados <i data-lucide="chevron-right" class="w-5 h-5 ml-1"></i>
                </button>
            </div>

            <!-- Passo 2: Dados -->
            <div id="step-2" class="hidden space-y-8 fade-in">
                <div class="premium-card p-8 rounded-[2.5rem]">
                    <button onclick="goToStep(1)" class="text-amber-200/40 text-[10px] font-black uppercase tracking-widest mb-10 flex items-center hover:text-amber-200">
                        <i data-lucide="chevron-left" class="w-4 h-4 mr-1"></i> Voltar aos horários
                    </button>
                    
                    <h2 class="text-3xl font-black mb-10 tracking-tighter gold-text">Seus Dados</h2>
                    
                    <div class="space-y-6">
                        <div>
                            <label class="block text-[10px] font-black text-amber-200/20 uppercase mb-2 ml-1">Nome Completo</label>
                            <div class="relative">
                                <i data-lucide="user" class="absolute left-5 top-1/2 -translate-y-1/2 w-5 h-5 text-amber-200/20"></i>
                                <input id="customer-name" type="text" placeholder="Nome completo" class="w-full pl-14 pr-6 py-5 bg-slate-950/60 border border-slate-800 rounded-2xl focus:border-amber-500/40 outline-none text-amber-100 placeholder:text-slate-800 font-bold">
                            </div>
                        </div>
                        <div>
                            <label class="block text-[10px] font-black text-amber-200/20 uppercase mb-2 ml-1">Telefone</label>
                            <div class="relative">
                                <i data-lucide="phone" class="absolute left-5 top-1/2 -translate-y-1/2 w-5 h-5 text-amber-200/20"></i>
                                <input id="customer-phone" type="tel" placeholder="(99) 99999-9999" class="w-full pl-14 pr-6 py-5 bg-slate-950/60 border border-slate-800 rounded-2xl focus:border-amber-500/40 outline-none text-amber-100 placeholder:text-slate-800 font-bold">
                            </div>
                        </div>
                        <button onclick="finishBooking()" class="w-full gold-gradient text-slate-950 py-5 rounded-2xl font-black text-xs uppercase tracking-[0.2em] shadow-xl hover:brightness-110 active:scale-95 transition-all mt-4">
                            Confirmar Reserva VIP
                        </button>
                    </div>
                </div>
            </div>

            <!-- Passo 3: Sucesso -->
            <div id="step-3" class="hidden space-y-6 fade-in">
                <div class="premium-card p-8 rounded-[3rem] relative overflow-hidden">
                    <div class="flex flex-col items-center mb-10">
                        <div class="p-5 rounded-full gold-gradient text-slate-950 mb-6 shadow-2xl">
                            <i data-lucide="check-circle" class="w-8 h-8"></i>
                        </div>
                        <h2 class="text-2xl font-black uppercase tracking-tighter text-center gold-text">Confirmado</h2>
                        <p class="text-amber-200/30 font-black text-[9px] uppercase tracking-[0.5em] mt-2">Grupo RODLIDER</p>
                    </div>

                    <!-- Comprovante Final -->
                    <div id="receipt-content" class="bg-slate-950/90 p-8 rounded-[2rem] border border-amber-900/20 text-amber-100/90 leading-relaxed shadow-inner font-sans">
                        <!-- Conteúdo preenchido via JS -->
                    </div>

                    <div class="grid grid-cols-1 gap-3 mt-10">
                        <button onclick="copyReceipt()" id="copy-btn" class="flex items-center justify-center py-5 rounded-2xl font-black text-[10px] uppercase tracking-[0.2em] gold-gradient text-slate-950 hover:brightness-110 shadow-lg transition-all">
                            <i data-lucide="copy" class="w-4 h-4 mr-2"></i> Copiar Comprovante
                        </button>
                        <button onclick="shareWhatsApp()" class="flex items-center justify-center py-5 rounded-2xl font-black text-[10px] uppercase tracking-[0.2em] bg-slate-900 text-amber-200/80 hover:bg-slate-800 border border-amber-900/20 transition-all">
                            <i data-lucide="share-2" class="w-4 h-4 mr-2"></i> Enviar WhatsApp
                        </button>
                    </div>
                </div>
                <button onclick="resetApp()" class="w-full py-4 text-slate-600 font-black text-[9px] uppercase tracking-[0.5em] hover:text-amber-200">Fazer Novo Agendamento</button>
            </div>
        </main>

        <!-- Footer -->
        <footer class="mt-24 pt-10 border-t border-slate-900/50">
            <div class="flex flex-col items-center opacity-40">
                <p class="text-[10px] font-black uppercase tracking-[0.3em] mb-2 gold-text">JURUCEY SOUSA BUSINESS CENTER</p>
                <div class="flex items-center text-[8px] font-bold text-slate-500 uppercase tracking-widest">
                    <i data-lucide="map-pin" class="w-2.5 h-2.5 mr-2 text-amber-600"></i> Santa Inês - Maranhão
                </div>
            </div>
        </footer>
    </div>

    <script>
        // Configurações
        const scheduleConfig = {
            weekday: { 
                am: ['08:00', '08:30', '09:00', '09:30', '10:00', '10:30', '11:00', '11:30'], 
                pm: ['14:00', '14:30', '15:00', '15:30', '16:00', '16:30', '17:00', '17:30'] 
            },
            saturday: { 
                am: ['09:00', '09:30', '10:00', '10:30', '11:00', '11:30'], 
                pm: [] 
            }
        };

        let selectedDate = new Date();
        let selectedTime = null;

        // Iniciar Lucide Icons
        function initIcons() {
            lucide.createIcons();
        }

        // Formatação de Data
        function formatDate(date) {
            return date.toLocaleDateString('pt-BR');
        }

        // Atualizar display de data e horários
        function updateView() {
            document.getElementById('current-date-display').innerText = formatDate(selectedDate);
            document.getElementById('prev-btn').disabled = selectedDate.toDateString() === new Date().toDateString();
            
            const day = selectedDate.getDay();
            const container = document.getElementById('time-grid-container');
            container.innerHTML = '';
            
            if (day === 0) { // Domingo
                container.innerHTML = `
                    <div class="bg-slate-900/40 border border-amber-900/20 p-12 rounded-[2.5rem] text-center">
                        <p class="text-xl font-bold gold-text">Domingo não há expediente</p>
                    </div>`;
                document.getElementById('next-step-btn').classList.add('hidden');
                return;
            }

            const slots = day === 6 ? scheduleConfig.saturday : scheduleConfig.weekday;
            
            let html = '';
            if (slots.am.length > 0) {
                html += `<div><p class="text-[10px] font-black text-amber-200/30 uppercase mb-4 tracking-[0.3em] ml-1">Período Manhã</p><div class="grid grid-cols-4 gap-3">`;
                slots.am.forEach(t => html += `<button onclick="selectTime('${t}')" class="time-slot py-4 rounded-2xl text-sm font-black border border-slate-800 bg-slate-900/40 transition-all ${selectedTime === t ? 'selected' : ''}">${t}</button>`);
                html += `</div></div>`;
            }
            if (slots.pm.length > 0) {
                html += `<div class="mt-8"><p class="text-[10px] font-black text-amber-200/30 uppercase mb-4 tracking-[0.3em] ml-1">Período Tarde</p><div class="grid grid-cols-4 gap-3">`;
                slots.pm.forEach(t => html += `<button onclick="selectTime('${t}')" class="time-slot py-4 rounded-2xl text-sm font-black border border-slate-800 bg-slate-900/40 transition-all ${selectedTime === t ? 'selected' : ''}">${t}</button>`);
                html += `</div></div>`;
            }
            
            container.innerHTML = html;
        }

        function selectTime(time) {
            selectedTime = time;
            updateView();
            document.getElementById('next-step-btn').classList.remove('hidden');
        }

        function changeDate(offset) {
            const nextDate = new Date(selectedDate);
            nextDate.setDate(selectedDate.getDate() + offset);
            if (nextDate < new Date().setHours(0,0,0,0)) return;
            selectedDate = nextDate;
            selectedTime = null;
            document.getElementById('next-step-btn').classList.add('hidden');
            updateView();
        }

        function goToStep(n) {
            document.getElementById('step-1').classList.add('hidden');
            document.getElementById('step-2').classList.add('hidden');
            document.getElementById('step-3').classList.add('hidden');
            document.getElementById(`step-${n}`).classList.remove('hidden');
            window.scrollTo(0,0);
        }

        function finishBooking() {
            const name = document.getElementById('customer-name').value;
            const phone = document.getElementById('customer-phone').value;
            
            if (!name || !phone) {
                return; // Em HTML real, usaria um modal customizado aqui
            }

            const receiptHtml = `
                <p class="font-black text-center mb-6 tracking-widest gold-text text-lg">*AGENDAMENTO PRESENCIAL*</p>
                <p><span class="font-bold">*Cliente:*</span> ${name}</p>
                <div class="py-2">
                    <p><span class="font-bold">*Horário:*</span> ${selectedTime}h</p>
                    <p><span class="font-bold">*Data:*</span> ${formatDate(selectedDate)}</p>
                </div>
                <div class="pt-4 border-t border-amber-900/10">
                    <p class="font-bold">*Endereço*:</p>
                    <p>Rua do Sol, 422 Centro</p>
                    <p>Santa Inês - MA</p>
                    <p>No Centro Empresarial Jurucey Sousa.</p>
                    <p class="text-amber-200/40 text-xs mt-1">Segundo Andar/Sala 10</p>
                </div>
            `;
            
            document.getElementById('receipt-content').innerHTML = receiptHtml;
            goToStep(3);
        }

        function getRawText() {
            const name = document.getElementById('customer-name').value;
            return `*AGENDAMENTO PRESENCIAL* \n\n*Cliente:* ${name}\n\n*Horário:* ${selectedTime}h\n*Data:* ${formatDate(selectedDate)}\n\n*Endereço*:\nRua do Sol, 422 Centro\nSanta Inês - MA \nNo Centro Empresarial Jurucey Sousa.\nSegundo Andar/Sala 10`;
        }

        function copyReceipt() {
            const text = getRawText();
            const el = document.createElement('textarea');
            el.value = text;
            document.body.appendChild(el);
            el.select();
            document.execCommand('copy');
            document.body.removeChild(el);
            
            const btn = document.getElementById('copy-btn');
            const original = btn.innerHTML;
            btn.innerHTML = 'Copiado!';
            btn.classList.replace('gold-gradient', 'bg-green-600');
            btn.classList.add('text-white');
            setTimeout(() => {
                btn.innerHTML = original;
                btn.classList.replace('bg-green-600', 'gold-gradient');
                btn.classList.remove('text-white');
            }, 2000);
        }

        function shareWhatsApp() {
            const text = encodeURIComponent(getRawText());
            window.open(`https://wa.me/?text=${text}`, '_blank');
        }

        function resetApp() {
            selectedTime = null;
            document.getElementById('customer-name').value = '';
            document.getElementById('customer-phone').value = '';
            goToStep(1);
            updateView();
        }

        // Inicialização
        window.onload = () => {
            initIcons();
            updateView();
        };
    </script>
</body>
</html>

