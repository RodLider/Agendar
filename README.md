import React, { useState } from 'react';
import { 
  Calendar as CalendarIcon, 
  Clock, 
  User, 
  Phone, 
  CheckCircle, 
  ChevronLeft, 
  ChevronRight,
  MapPin,
  Copy,
  Share2,
  Building2,
  Crown
} from 'lucide-react';

const App = () => {
  const [selectedDate, setSelectedDate] = useState(new Date());
  const [selectedTime, setSelectedTime] = useState(null);
  const [step, setStep] = useState(1);
  const [formData, setFormData] = useState({ name: '', phone: '' });
  const [copied, setCopied] = useState(false);

  // Estilos Dourados Premium
  const goldGradient = "bg-gradient-to-r from-[#bf953f] via-[#fcf6ba] to-[#aa771c]";
  const goldText = "bg-gradient-to-r from-[#bf953f] via-[#fcf6ba] to-[#aa771c] bg-clip-text text-transparent";
  const goldBorder = "border-[#bf953f]/40";

  // Configuração de Horários (Conforme solicitado)
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

  const isSunday = (date) => date.getDay() === 0;
  const isSaturday = (date) => date.getDay() === 6;

  const getAvailableSlots = (date) => {
    if (isSunday(date)) return [];
    if (isSaturday(date)) return scheduleConfig.saturday.am;
    return [...scheduleConfig.weekday.am, ...scheduleConfig.weekday.pm];
  };

  const handleDateChange = (offset) => {
    const newDate = new Date(selectedDate);
    newDate.setDate(selectedDate.getDate() + offset);
    const today = new Date();
    today.setHours(0,0,0,0);
    if (newDate < today) return;
    setSelectedDate(newDate);
    setSelectedTime(null);
  };

  const formatDisplayDate = (date) => {
    return new Intl.DateTimeFormat('pt-BR', { 
      day: '2-digit', 
      month: '2-digit', 
      year: 'numeric' 
    }).format(date);
  };

  // MODELO DE TEXTO SOLICITADO
  const getSummaryText = () => {
    return `*AGENDAMENTO PRESENCIAL* \n\n*Cliente:* ${formData.name}\n\n*Horário:* ${selectedTime}h\n*Data:* ${formatDisplayDate(selectedDate)}\n\n*Endereço*:\nRua do Sol, 422 Centro\nSanta Inês - MA \nNo Centro Empresarial Jurucey Sousa.\nSegundo Andar/Sala 10`;
  };

  const copyToClipboard = () => {
    const text = getSummaryText();
    const textArea = document.createElement("textarea");
    textArea.value = text;
    document.body.appendChild(textArea);
    textArea.select();
    try {
      document.execCommand('copy');
      setCopied(true);
      setTimeout(() => setCopied(false), 2000);
    } catch (err) {
      console.error('Falha ao copiar', err);
    }
    document.body.removeChild(textArea);
  };

  const renderStep1 = () => {
    const slots = getAvailableSlots(selectedDate);
    const amSlots = slots.filter(s => parseInt(s.split(':')[0]) < 13);
    const pmSlots = slots.filter(s => parseInt(s.split(':')[0]) >= 13);

    return (
      <div className="space-y-8 animate-in fade-in duration-700">
        <div className={`bg-slate-900/60 p-6 rounded-[2rem] border ${goldBorder} backdrop-blur-md shadow-2xl`}>
          <div className="flex items-center justify-between">
            <button onClick={() => handleDateChange(-1)} className="p-3 text-amber-200/40 hover:text-amber-200 disabled:opacity-5 transition-colors" disabled={selectedDate.toDateString() === new Date().toDateString()}>
              <ChevronLeft className="w-8 h-8" />
            </button>
            <div className="text-center">
              <h2 className={`text-2xl font-black tracking-tight ${goldText}`}>
                {formatDisplayDate(selectedDate)}
              </h2>
              <p className="text-[10px] text-amber-100/30 font-bold uppercase tracking-[0.4em] mt-1">Selecione o Dia</p>
            </div>
            <button onClick={() => handleDateChange(1)} className="p-3 text-amber-200/40 hover:text-amber-200 transition-colors">
              <ChevronRight className="w-8 h-8" />
            </button>
          </div>
        </div>

        {!isSunday(selectedDate) ? (
          <div className="space-y-8">
            {amSlots.length > 0 && (
              <div className="animate-in slide-in-from-left duration-500">
                <p className="text-[10px] font-black text-amber-200/30 uppercase mb-4 flex items-center px-1 tracking-[0.3em] ml-1">
                  <Clock className="w-3 h-3 mr-2" /> Período Manhã
                </p>
                <div className="grid grid-cols-4 gap-3">
                  {amSlots.map(t => (
                    <button 
                      key={t} 
                      onClick={() => setSelectedTime(t)} 
                      className={`py-4 rounded-2xl text-sm font-black border transition-all duration-300 ${
                        selectedTime === t 
                        ? `${goldGradient} text-slate-950 border-transparent shadow-lg shadow-amber-500/30 scale-105` 
                        : `bg-slate-900/40 border-slate-800 text-amber-100/60 hover:${goldBorder}`
                      }`}
                    >
                      {t}
                    </button>
                  ))}
                </div>
              </div>
            )}
            {pmSlots.length > 0 && (
              <div className="animate-in slide-in-from-right duration-500">
                <p className="text-[10px] font-black text-amber-200/30 uppercase mb-4 flex items-center px-1 tracking-[0.3em] ml-1">
                  <Clock className="w-3 h-3 mr-2" /> Período Tarde
                </p>
                <div className="grid grid-cols-4 gap-3">
                  {pmSlots.map(t => (
                    <button 
                      key={t} 
                      onClick={() => setSelectedTime(t)} 
                      className={`py-4 rounded-2xl text-sm font-black border transition-all duration-300 ${
                        selectedTime === t 
                        ? `${goldGradient} text-slate-950 border-transparent shadow-lg shadow-amber-500/30 scale-105` 
                        : `bg-slate-900/40 border-slate-800 text-amber-100/60 hover:${goldBorder}`
                      }`}
                    >
                      {t}
                    </button>
                  ))}
                </div>
              </div>
            )}
          </div>
        ) : (
          <div className="bg-slate-900/40 border border-amber-900/20 p-12 rounded-[2.5rem] text-center">
            <p className={`text-xl font-bold ${goldText}`}>Domingo não há expediente</p>
          </div>
        )}

        {selectedTime && (
          <button 
            onClick={() => setStep(2)} 
            className={`w-full ${goldGradient} text-slate-950 py-5 rounded-2xl font-black text-xs uppercase tracking-[0.2em] shadow-2xl hover:brightness-110 active:scale-95 transition-all flex items-center justify-center`}
          >
            Continuar para Dados <ChevronRight className="w-5 h-5 ml-1" />
          </button>
        )}
      </div>
    );
  };

  const renderStep2 = () => (
    <div className={`bg-slate-900/40 p-8 rounded-[2.5rem] border ${goldBorder} backdrop-blur-xl animate-in slide-in-from-bottom-8 duration-500`}>
      <button onClick={() => setStep(1)} className="text-amber-200/40 text-[10px] font-black uppercase tracking-widest mb-10 flex items-center hover:text-amber-200 transition-colors">
        <ChevronLeft className="w-4 h-4 mr-1" /> Voltar aos horários
      </button>
      
      <h2 className={`text-3xl font-black mb-10 tracking-tighter ${goldText}`}>Seus Dados</h2>
      
      <div className="space-y-6">
        <div>
          <label className="block text-[10px] font-black text-amber-200/20 uppercase mb-2 ml-1 tracking-[0.2em]">Nome Completo</label>
          <div className="relative">
            <User className="absolute left-5 top-1/2 -translate-y-1/2 w-5 h-5 text-amber-200/20" />
            <input 
              required 
              type="text" 
              placeholder="Digite seu nome..." 
              className="w-full pl-14 pr-6 py-5 bg-slate-950/60 border border-slate-800 rounded-2xl focus:border-amber-500/40 outline-none transition-all text-amber-100 placeholder:text-slate-800 font-bold"
              value={formData.name} 
              onChange={(e) => setFormData({...formData, name: e.target.value})} 
            />
          </div>
        </div>
        <div>
          <label className="block text-[10px] font-black text-amber-200/20 uppercase mb-2 ml-1 tracking-[0.2em]">Telefone de Contato</label>
          <div className="relative">
            <Phone className="absolute left-5 top-1/2 -translate-y-1/2 w-5 h-5 text-amber-200/20" />
            <input 
              required 
              type="tel" 
              placeholder="(00) 0 0000-0000" 
              className="w-full pl-14 pr-6 py-5 bg-slate-950/60 border border-slate-800 rounded-2xl focus:border-amber-500/40 outline-none transition-all text-amber-100 placeholder:text-slate-800 font-bold"
              value={formData.phone} 
              onChange={(e) => setFormData({...formData, phone: e.target.value})} 
            />
          </div>
        </div>
        
        <div className="pt-8">
          <button 
            onClick={() => formData.name && formData.phone && setStep(3)} 
            className={`w-full ${goldGradient} text-slate-950 py-5 rounded-2xl font-black text-xs uppercase tracking-[0.2em] shadow-xl hover:brightness-110 active:scale-95 transition-all`}
          >
            Confirmar Reserva VIP
          </button>
        </div>
      </div>
    </div>
  );

  const renderStep3 = () => (
    <div className="animate-in zoom-in duration-500 space-y-6">
      <div className={`bg-slate-900/60 p-8 rounded-[3rem] border ${goldBorder} backdrop-blur-2xl shadow-2xl relative`}>
        <div className="flex flex-col items-center mb-10">
          <div className={`p-5 rounded-full ${goldGradient} text-slate-950 mb-6`}>
            <CheckCircle className="w-8 h-8" />
          </div>
          <h2 className={`text-2xl font-black uppercase tracking-tighter text-center ${goldText}`}>Agendamento Concluído</h2>
          <p className="text-amber-200/30 font-black text-[9px] uppercase tracking-[0.5em] mt-2">Grupo RODLIDER</p>
        </div>

        {/* COMPROVANTE EXATO NO MODELO SOLICITADO */}
        <div className="bg-slate-950/90 p-8 rounded-[2rem] border border-amber-900/20 text-amber-100/90 leading-relaxed shadow-inner">
           <p className={`font-black text-center mb-6 tracking-widest ${goldText}`}>*AGENDAMENTO PRESENCIAL*</p>
           
           <div className="font-sans space-y-4">
             <p><span className="font-bold">*Cliente:*</span> {formData.name}</p>

             <div className="py-2">
               <p><span className="font-bold">*Horário:*</span> {selectedTime}h</p>
               <p><span className="font-bold">*Data:*</span> {formatDisplayDate(selectedDate)}</p>
             </div>
             
             <div className="pt-4 border-t border-amber-900/10">
               <p className="font-bold">*Endereço*:</p>
               <p>Rua do Sol, 422 Centro</p>
               <p>Santa Inês - MA</p>
               <p>No Centro Empresarial Jurucey Sousa.</p>
               <p className="text-amber-200/40 text-xs mt-1">Segundo Andar/Sala 10</p>
             </div>
           </div>
        </div>

        <div className="grid grid-cols-1 gap-3 mt-10">
          <button 
            onClick={copyToClipboard}
            className={`flex items-center justify-center py-5 rounded-2xl font-black text-[10px] uppercase tracking-[0.2em] transition-all ${copied ? 'bg-green-600 text-white' : `${goldGradient} text-slate-950 hover:brightness-110 shadow-lg shadow-amber-500/20`}`}
          >
            {copied ? <>Copiado com Sucesso!</> : <><Copy className="w-4 h-4 mr-2" /> Copiar Comprovante</>}
          </button>
          
          <button 
            onClick={() => window.open(`https://wa.me/?text=${encodeURIComponent(getSummaryText())}`, '_blank')}
            className="flex items-center justify-center py-5 rounded-2xl font-black text-[10px] uppercase tracking-[0.2em] bg-slate-900 text-amber-200/80 hover:bg-slate-800 border border-amber-900/20 transition-all"
          >
            <Share2 className="w-4 h-4 mr-2" /> Enviar para WhatsApp
          </button>
        </div>
      </div>

      <button onClick={() => {setStep(1); setSelectedTime(null); setFormData({name:'', phone:''})}} className="w-full py-4 text-slate-600 font-black text-[9px] uppercase tracking-[0.5em] hover:text-amber-200 transition-colors">Fazer Novo Agendamento</button>
    </div>
  );

  return (
    <div className="min-h-screen bg-[#020617] p-4 md:p-12 font-sans text-slate-100 selection:bg-amber-500/30">
      <div className="max-w-md mx-auto">
        {/* Header Premium */}
        <header className="flex flex-col items-center mb-16 text-center">
          <div className={`w-16 h-16 ${goldGradient} rounded-2xl flex items-center justify-center shadow-2xl shadow-amber-500/20 mb-6 rotate-3 transform hover:rotate-0 transition-all duration-500`}>
            <Building2 className="text-slate-950 w-8 h-8" />
          </div>
          <div className="space-y-1">
            <h1 className={`text-3xl font-black tracking-tighter uppercase leading-none ${goldText}`}>AGENDAMENTO</h1>
            <p className="text-amber-200/30 font-black text-[10px] uppercase tracking-[0.6em] flex items-center justify-center">
               <Crown className="w-3 h-3 mr-2" /> Grupo RODLIDER
            </p>
          </div>
        </header>

        <main className="relative">
          {step === 1 && renderStep1()}
          {step === 2 && renderStep2()}
          {step === 3 && renderStep3()}
        </main>

        <footer className="mt-24 pt-10 border-t border-slate-900/50">
          <div className="flex flex-col items-center opacity-40">
            <p className={`text-[10px] font-black uppercase tracking-[0.3em] mb-2 ${goldText}`}>
              GRUPO RODLIDER CONSULTORIA FINANCEIRA
            </p>
            <div className="flex items-center text-[8px] font-bold text-slate-500 uppercase tracking-widest">
              <MapPin className="w-2.5 h-2.5 mr-2 text-amber-600" /> Santa Inês - Maranhão
            </div>
          </div>
        </footer>
      </div>
    </div>
  );
};

export default App;

