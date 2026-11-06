import React, { useState, useEffect } from 'react';
import { initializeApp } from 'firebase/app';
import { 
  getAuth, 
  signInAnonymously, 
  signInWithCustomToken, 
  onAuthStateChanged 
} from 'firebase/auth';
import { 
  getFirestore, 
  collection, 
  onSnapshot, 
  addDoc, 
  updateDoc, 
  deleteDoc, 
  doc, 
  serverTimestamp 
} from 'firebase/firestore';
import { 
  Plus, Trash2, Printer, Save, FileSpreadsheet, Search, RefreshCw, 
  FileText, Users, Sparkles, Edit3, Download, X, Eye, CreditCard
} from 'lucide-react';

// --- Firebase Config Setup ---
const firebaseConfigStr = typeof __firebase_config !== 'undefined' ? __firebase_config : null;
let app, auth, db;
const appId = typeof __app_id !== 'undefined' ? __app_id : 'yunduo-quote-app';

if (firebaseConfigStr) {
  try {
    const firebaseConfig = JSON.parse(firebaseConfigStr);
    app = initializeApp(firebaseConfig);
    auth = getAuth(app);
    db = getFirestore(app);
  } catch (e) {
    console.error("Firebase init failed", e);
  }
}

// Pre-loaded photo quotation context
const DEFAULT_QUOTE = {
  projectName: '裕信路',
  phone: '0912-345-678',
  address: '臺南市東區裕信路',
  quoteDate: '115/11/06',
  paymentMethod: '現金/匯款/即期支票',
  salesRep: 'Bella',
  items: [
    { id: '1', category: '正面彎軌', name: '100%物理遮光素色布', qty: 66, price: 440 },
    { id: '2', category: '彎軌正面軌道', name: '緹花面無縫布', qty: 66, price: 440 },
    { id: '3', category: '房間', name: '壓花80%遮光布', qty: 12, price: 160 },
    { id: '4', category: '軌道', name: '鋁合金M6手撥軌道', qty: 5, price: 70 },
    { id: '5', category: '高空', name: '高空梯含安裝費用', qty: 1, price: 3500 },
    { id: '6', category: '', name: '免高空費用/同社區介紹', qty: '', price: '' },
    { id: '7', category: '', name: '優惠價格已扣除高空費用', qty: '', price: '' },
  ],
  customDiscountedTotal: 54000,
  salesSignature: 'Bella'
};

const BANK_INFO = "新光銀行 - 新營分行（代碼：103）｜ 戶名：畇朵家飾布企業社 ｜ 帳號：0541-10-1004306";
const TERMS_TEXT = [
  "付款方式：僅提供現金、匯款或即期支票。",
  "收款：訂單確認後，須回簽本報價單並支付訂金50%，以利安排生產與施工時程，完工收50%。",
  "客製商品說明：本報價單所列項目皆為客製化訂製品，恕不接受退訂或更改，如有尺寸、樣式、數量變更需重新報價。",
  "所有權保留：於貨款未全數付清前（含追加款項），相關產品及施工物品之所有權仍歸本公司所有。",
  "報價有效期限制：本報價單自開立日起有效14天，逾期本公司保有調整價格之權利。"
];

export default function App() {
  const [user, setUser] = useState(null);
  const [activeTab, setActiveTab] = useState('editor');
  const [currentDocId, setCurrentDocId] = useState(null);
  const [quoteData, setQuoteData] = useState(DEFAULT_QUOTE);
  const [savedQuotes, setSavedQuotes] = useState([]);
  const [searchTerm, setSearchTerm] = useState('');
  const [loading, setLoading] = useState(false);
  const [notification, setNotification] = useState('');
  
  // NEW: Print Modal Overlay state
  const [showPrintModal, setShowPrintModal] = useState(false);

  const showToast = (msg) => {
    setNotification(msg);
    setTimeout(() => setNotification(''), 3500);
  };

  // Auth setup
  useEffect(() => {
    if (!auth) return;
    const initAuth = async () => {
      try {
        if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
          await signInWithCustomToken(auth, __initial_auth_token);
        } else {
          await signInAnonymously(auth);
        }
      } catch (err) {
        console.error("Auth error:", err);
      }
    };
    initAuth();
    const unsubscribe = onAuthStateChanged(auth, (u) => setUser(u));
    return () => unsubscribe();
  }, []);

  // Fetch Firestore Realtime Data + LocalStorage Backup
  useEffect(() => {
    // 1. Try Loading from LocalStorage first as initial fallback
    const localSaved = localStorage.getItem('yunduo_quotes');
    if (localSaved) {
      try {
        setSavedQuotes(JSON.parse(localSaved));
      } catch (e) {}
    }

    // 2. Load Firestore Data if online
    if (!user || !db) return;
    setLoading(true);
    const quotesCol = collection(db, 'artifacts', appId, 'public', 'data', 'quotations');
    
    const unsubscribe = onSnapshot(quotesCol, (snapshot) => {
      const data = snapshot.docs.map(doc => ({
        id: doc.id,
        ...doc.data()
      }));
      data.sort((a, b) => (b.createdAt?.seconds || 0) - (a.createdAt?.seconds || 0));
      setSavedQuotes(data);
      localStorage.setItem('yunduo_quotes', JSON.stringify(data));
      setLoading(false);
    }, (error) => {
      console.error("Error fetching quotes:", error);
      setLoading(false);
    });

    return () => unsubscribe();
  }, [user]);

  // Calculations
  const calculateTotals = () => {
    let rawTotal = 0;
    (quoteData.items || []).forEach(item => {
      const q = parseFloat(item.qty) || 0;
      const p = parseFloat(item.price) || 0;
      rawTotal += q * p;
    });
    const finalPrice = quoteData.customDiscountedTotal !== '' && quoteData.customDiscountedTotal !== undefined 
      ? Number(quoteData.customDiscountedTotal) 
      : rawTotal;
    const depositAmount = Math.round(finalPrice * 0.5);

    return { rawTotal, finalPrice, depositAmount };
  };

  const { rawTotal, finalPrice, depositAmount } = calculateTotals();

  // Handlers for Items
  const handleItemChange = (index, field, value) => {
    const newItems = [...quoteData.items];
    newItems[index][field] = value;
    setQuoteData({ ...quoteData, items: newItems });
  };

  const addItemRow = () => {
    const newItem = {
      id: Date.now().toString(),
      category: '',
      name: '',
      qty: '',
      price: ''
    };
    setQuoteData({
      ...quoteData,
      items: [...quoteData.items, newItem]
    });
  };

  const removeItemRow = (index) => {
    if (quoteData.items.length <= 1) {
      showToast('至少保留一個品項');
      return;
    }
    const newItems = quoteData.items.filter((_, i) => i !== index);
    setQuoteData({ ...quoteData, items: newItems });
  };

  // Save Quote
  const handleSaveToCloud = async () => {
    const payload = {
      ...quoteData,
      rawTotal,
      finalPrice,
      depositAmount,
      updatedAt: new Date().toISOString()
    };

    const localId = currentDocId || 'local_' + Date.now();
    const updatedLocal = [...savedQuotes.filter(q => q.id !== localId), { ...payload, id: localId }];
    setSavedQuotes(updatedLocal);
    localStorage.setItem('yunduo_quotes', JSON.stringify(updatedLocal));

    if (!db || !user) {
      setCurrentDocId(localId);
      showToast('✅ 已儲存至本機紀錄');
      return;
    }

    try {
      setLoading(true);
      const quotesCol = collection(db, 'artifacts', appId, 'public', 'data', 'quotations');
      if (currentDocId && !currentDocId.startsWith('local_')) {
        const docRef = doc(db, 'artifacts', appId, 'public', 'data', 'quotations', currentDocId);
        await updateDoc(docRef, { ...payload, updatedAt: serverTimestamp() });
        showToast('✅ 雲端報價單更新成功！');
      } else {
        const docRef = await addDoc(quotesCol, { ...payload, createdAt: serverTimestamp() });
        setCurrentDocId(docRef.id);
        showToast('✅ 成功儲存新報價單至雲端！');
      }
    } catch (e) {
      console.error("Save error:", e);
      showToast('⚠️ 雲端保存提示：已儲存於本機');
    } finally {
      setLoading(false);
    }
  };

  const handleCreateNew = () => {
    setCurrentDocId(null);
    setQuoteData({
      projectName: '',
      phone: '',
      address: '',
      quoteDate: new Date().toLocaleDateString('zh-TW', { year: 'numeric', month: '2-digit', day: '2-digit' }),
      paymentMethod: '現金/匯款/即期支票',
      salesRep: 'Bella',
      items: [
        { id: Date.now().toString(), category: '', name: '', qty: 1, price: 0 }
      ],
      customDiscountedTotal: 0,
      salesSignature: 'Bella'
    });
    setActiveTab('editor');
    showToast('已建立新空白報價單');
  };

  const handleLoadQuote = (item) => {
    setCurrentDocId(item.id);
    setQuoteData(item);
    setActiveTab('editor');
    showToast(`已載入：${item.projectName || '未命名'}`);
  };

  const handleDeleteQuote = async (id, e) => {
    e.stopPropagation();
    if (!window.confirm('確定要刪除這筆報價單紀錄嗎？')) return;
    
    const updatedLocal = savedQuotes.filter(q => q.id !== id);
    setSavedQuotes(updatedLocal);
    localStorage.setItem('yunduo_quotes', JSON.stringify(updatedLocal));

    if (db && user && !id.startsWith('local_')) {
      try {
        const docRef = doc(db, 'artifacts', appId, 'public', 'data', 'quotations', id);
        await deleteDoc(docRef);
      } catch (e) {}
    }

    if (currentDocId === id) handleCreateNew();
    showToast('已成功刪除紀錄');
  };

  // --- 🌟 EXPORT PDF & DOWNLOAD HTML SOLUTIONS 🌟 ---

  // Solution 1: Direct System Print
  const handleSystemPrint = () => {
    try {
      window.print();
    } catch (err) {
      showToast('⚠️ 系統呼叫列印受限，請使用右側【下載獨立列印檔】');
    }
  };

  // Solution 2: Download Pure Standalone HTML File
  const handleDownloadStandaloneHTML = () => {
    const itemsRows = (quoteData.items || []).map(item => {
      const q = parseFloat(item.qty);
      const p = parseFloat(item.price);
      const sub = (!isNaN(q) && !isNaN(p)) ? (q * p).toLocaleString() : '';
      return `
        <tr>
          <td style="border:1px solid #000; padding:6px; text-align:center;">${item.category || ''}</td>
          <td style="border:1px solid #000; padding:6px; text-align:left;">${item.name || ''}</td>
          <td style="border:1px solid #000; padding:6px; text-align:center;">${item.qty ?? ''}</td>
          <td style="border:1px solid #000; padding:6px; text-align:right; font-family:monospace;">${item.price ?? ''}</td>
          <td style="border:1px solid #000; padding:6px; text-align:right; font-family:monospace; font-weight:bold;">${sub}</td>
        </tr>
      `;
    }).join('');

    const htmlContent = `
      <!DOCTYPE html>
      <html lang="zh-TW">
      <head>
        <meta charset="UTF-8">
        <title>畇朵報價單_${quoteData.projectName || '客戶'}</title>
        <style>
          @page { size: A4 portrait; margin: 10mm; }
          body { font-family: "PingFang TC", "Microsoft JhengHei", sans-serif; background: #fff; color: #000; padding: 20px; max-width: 210mm; margin: 0 auto; }
          .header { text-align: center; border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 16px; }
          .header h1 { margin: 0; font-size: 24px; letter-spacing: 4px; }
          .info-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; border: 1px solid #000; padding: 12px; margin-bottom: 16px; font-size: 13px; }
          table { width: 100%; border-collapse: collapse; margin-bottom: 16px; font-size: 13px; }
          th { background-color: #c8b284; border: 1px solid #000; padding: 8px; font-weight: bold; }
          .terms-box { display: flex; border: 1px solid #000; margin-bottom: 16px; font-size: 11px; }
          .terms-left { flex: 7; padding: 10px; border-right: 1px solid #000; }
          .terms-right { flex: 5; display: flex; flex-direction: column; }
          .total-row { display: flex; height: 50%; border-bottom: 1px solid #000; }
          .total-row:last-child { border-bottom: none; }
          .total-label { width: 35%; background: #c8b284; font-weight: bold; display: flex; align-items: center; justify-content: center; border-right: 1px solid #000; }
          .total-val { width: 65%; display: flex; align-items: center; justify-content: flex-end; padding-right: 10px; font-weight: bold; font-family: monospace; font-size: 15px; }
          .bank-info { font-size: 11px; padding: 6px; background: #f9f9f9; border: 1px solid #ccc; margin-bottom: 24px; text-align: center; }
          .signatures { display: flex; justify-content: space-around; text-align: center; margin-top: 20px; }
          .sig-line { border-bottom: 1px solid #000; width: 160px; height: 30px; margin-bottom: 4px; font-size: 16px; font-weight: bold; line-height: 35px; }
          @media print {
            .no-print { display: none !important; }
            body { padding: 0; }
          }
        </style>
      </head>
      <body>
        <div class="no-print" style="background: #fff3cd; color: #856404; padding: 12px; margin-bottom: 20px; border-radius: 6px; border: 1px solid #ffeeba; text-align: center; font-size: 14px;">
          💡 <strong>列印提示：</strong>請直接按下鍵盤 <kbd>Ctrl + P</kbd> 或 <kbd>Cmd + P</kbd>，在彈出視窗選擇「另存為 PDF」即可！
        </div>

        <div class="header">
          <h1>畇朵報價單</h1>
        </div>

        <div class="info-grid">
          <div><strong>案名：</strong>${quoteData.projectName || ''}</div>
          <div><strong>報價日期：</strong>${quoteData.quoteDate || ''}</div>
          <div><strong>聯絡電話：</strong>${quoteData.phone || ''}</div>
          <div><strong>付款方式：</strong>${quoteData.paymentMethod || ''}</div>
          <div><strong>地址：</strong>${quoteData.address || ''}</div>
          <div><strong>承辦業務：</strong>${quoteData.salesRep || ''}</div>
        </div>

        <table>
          <thead>
            <tr>
              <th style="width:18%;">項目</th>
              <th style="width:40%;">品名</th>
              <th style="width:12%;">數量</th>
              <th style="width:15%;">單價</th>
              <th style="width:15%;">金額</th>
            </tr>
          </thead>
          <tbody>
            ${itemsRows}
          </tbody>
        </table>

        <div class="terms-box">
          <div class="terms-left">
            <strong>為確保雙方權益及服務品質，相關合作條款如下：</strong>
            <ol style="margin: 4px 0 0 16px; padding: 0;">
              ${TERMS_TEXT.map(t => `<li style="margin-bottom:2px;">${t}</li>`).join('')}
            </ol>
          </div>
          <div class="terms-right">
            <div class="total-row">
              <div class="total-label">總計</div>
              <div class="total-val">$${rawTotal.toLocaleString()}</div>
            </div>
            <div class="total-row">
              <div class="total-label">優惠價含稅</div>
              <div class="total-val" style="color: #8c6239;">$${(quoteData.customDiscountedTotal || rawTotal).toLocaleString()}</div>
            </div>
          </div>
        </div>

        <div class="bank-info">
          <strong>【匯款資訊】</strong> ${BANK_INFO}
        </div>

        <div class="signatures">
          <div>
            <div class="sig-line">${quoteData.salesSignature || quoteData.salesRep || ''}</div>
            <div style="font-size:12px; font-weight:bold;">公司業務簽章</div>
          </div>
          <div>
            <div class="sig-line"></div>
            <div style="font-size:12px; font-weight:bold;">客戶簽章</div>
          </div>
        </div>

        <script>
          // Auto trigger print when opened directly
          window.onload = function() {
            setTimeout(() => { window.print(); }, 400);
          }
        </script>
      </body>
      </html>
    `;

    const blob = new Blob([htmlContent], { type: 'text/html;charset=utf-8;' });
    const url = URL.createObjectURL(blob);
    const link = document.createElement('a');
    link.setAttribute('href', url);
    link.setAttribute('download', `畇朵報價單_${quoteData.projectName || '客戶'}.html`);
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
    showToast('✅ 已下載獨立報價單檔案，打開即可另存 PDF！');
  };

  // Export CSV
  const handleExportCSV = () => {
    if (savedQuotes.length === 0) {
      showToast('目前尚無報價資料可匯出');
      return;
    }

    const headers = ['案名', '報價日期', '承辦業務', '聯絡電話', '地址', '試算總額', '優惠成交價', '付款方式'];
    const rows = savedQuotes.map(q => [
      `"${q.projectName || ''}"`,
      `"${q.quoteDate || ''}"`,
      `"${q.salesRep || ''}"`,
      `"${q.phone || ''}"`,
      `"${q.address || ''}"`,
      q.rawTotal || 0,
      q.finalPrice || 0,
      `"${q.paymentMethod || ''}"`
    ]);

    const csvContent = "\uFEFF" + [headers.join(','), ...rows.map(e => e.join(','))].join('\n');
    const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
    const url = URL.createObjectURL(blob);
    const link = document.createElement('a');
    link.setAttribute('href', url);
    link.setAttribute('download', `畇朵家飾_客戶報價備份資料_${new Date().toISOString().slice(0,10)}.csv`);
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
    showToast('已匯出 Excel / CSV 彙整表');
  };

  const filteredQuotes = savedQuotes.filter(q => {
    const term = searchTerm.toLowerCase();
    return (
      (q.projectName && q.projectName.toLowerCase().includes(term)) ||
      (q.salesRep && q.salesRep.toLowerCase().includes(term)) ||
      (q.phone && q.phone.includes(term)) ||
      (q.address && q.address.toLowerCase().includes(term))
    );
  });

  return (
    <div className="min-h-screen bg-stone-100 text-stone-800 font-sans print:bg-white print:p-0">
      
      {/* Toast Notification */}
      {notification && (
        <div className="fixed top-4 right-4 z-50 bg-amber-900 text-amber-50 px-4 py-2.5 rounded-lg shadow-xl border border-amber-700/50 flex items-center space-x-2 animate-bounce print:hidden">
          <Sparkles className="w-4 h-4 text-amber-300" />
          <span className="text-sm font-medium">{notification}</span>
        </div>
      )}

      {/* Navigation Header */}
      <header className="bg-stone-900 text-stone-100 shadow-md print:hidden sticky top-0 z-40">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
          <div className="flex items-center space-x-3">
            <div className="w-9 h-9 rounded-lg bg-gradient-to-br from-amber-600 to-amber-800 flex items-center justify-center text-white font-bold text-xl shadow-inner">
              畇
            </div>
            <div>
              <h1 className="font-semibold text-base sm:text-lg tracking-wide text-stone-100">畇朵家飾 報價與客戶管理系統</h1>
              <p className="text-xs text-amber-200/70 hidden sm:block">Yunduo Home Furnishings Quotation & CRM</p>
            </div>
          </div>

          <div className="flex items-center space-x-2 sm:space-x-3">
            <button
              onClick={() => setActiveTab('editor')}
              className={`px-3 py-1.5 sm:px-4 sm:py-2 text-xs sm:text-sm font-medium rounded-lg transition flex items-center space-x-1.5 ${
                activeTab === 'editor' 
                  ? 'bg-amber-700 text-amber-50 shadow-sm' 
                  : 'text-stone-300 hover:bg-stone-800 hover:text-white'
              }`}
            >
              <FileText className="w-4 h-4" />
              <span>報價單編輯器</span>
            </button>

            <button
              onClick={() => setActiveTab('dashboard')}
              className={`px-3 py-1.5 sm:px-4 sm:py-2 text-xs sm:text-sm font-medium rounded-lg transition flex items-center space-x-1.5 ${
                activeTab === 'dashboard' 
                  ? 'bg-amber-700 text-amber-50 shadow-sm' 
                  : 'text-stone-300 hover:bg-stone-800 hover:text-white'
              }`}
            >
              <Users className="w-4 h-4" />
              <span>客戶歷史紀錄</span>
              {savedQuotes.length > 0 && (
                <span className="ml-1 bg-amber-900 text-amber-200 text-xs px-1.5 py-0.5 rounded-full font-mono">
                  {savedQuotes.length}
                </span>
              )}
            </button>
          </div>
        </div>
      </header>

      {/* Main Content */}
      <main className="max-w-7xl mx-auto px-2 sm:px-6 lg:px-8 py-4 sm:py-8 print:p-0 print:max-w-none">
        
        {/* --- EDITOR & PRINT VIEW TAB --- */}
        {activeTab === 'editor' && (
          <div className="space-y-6">
            
            {/* Control Toolbar (Hidden in Print) */}
            <div className="bg-white rounded-xl shadow-sm border border-stone-200 p-4 flex flex-wrap items-center justify-between gap-3 print:hidden">
              <div className="flex items-center space-x-2">
                <button
                  onClick={handleCreateNew}
                  className="px-3.5 py-2 bg-stone-100 hover:bg-stone-200 text-stone-700 rounded-lg text-xs sm:text-sm font-medium transition flex items-center space-x-1.5"
                >
                  <Plus className="w-4 h-4" />
                  <span>開新報價單</span>
                </button>
                {currentDocId && (
                  <span className="text-xs bg-amber-50 text-amber-800 px-2.5 py-1 rounded border border-amber-200">
                    目前編輯：{quoteData.projectName || '未命名'}
                  </span>
                )}
              </div>

              <div className="flex items-center space-x-2 sm:space-x-3">
                <button
                  onClick={handleSaveToCloud}
                  disabled={loading}
                  className="px-4 py-2 bg-amber-800 hover:bg-amber-900 text-white rounded-lg text-xs sm:text-sm font-medium transition flex items-center space-x-1.5 shadow-sm disabled:opacity-50"
                >
                  <Save className="w-4 h-4" />
                  <span>{loading ? '儲存中...' : '儲存紀錄'}</span>
                </button>

                {/* Print / Save PDF Main Button */}
                <button
                  onClick={() => setShowPrintModal(true)}
                  className="px-4 py-2 bg-stone-900 hover:bg-black text-white rounded-lg text-xs sm:text-sm font-medium transition flex items-center space-x-1.5 shadow-sm"
                >
                  <Eye className="w-4 h-4" />
                  <span>列印 / 匯出 PDF 預覽</span>
                </button>

                {/* Download Standalone Print HTML File (Fallback) */}
                <button
                  onClick={handleDownloadStandaloneHTML}
                  title="匯出獨立 HTML 列印檔（雙擊即可列印另存 PDF）"
                  className="px-3 py-2 bg-stone-100 hover:bg-stone-200 text-stone-700 rounded-lg text-xs font-medium transition flex items-center space-x-1"
                >
                  <Download className="w-3.5 h-3.5" />
                  <span className="hidden sm:inline">下載獨立列印檔 (.html)</span>
                </button>
              </div>
            </div>

            {/* A4 Document Sheet */}
            <div 
              id="printable-quote"
              className="bg-white shadow-xl print:shadow-none rounded-none sm:rounded-lg overflow-hidden border border-stone-200 print:border-0 max-w-[210mm] mx-auto p-4 sm:p-10 print:p-0 my-0 print:my-0"
            >
              
              {/* Header Title */}
              <div className="text-center pb-3 mb-4 border-b-2 border-stone-900">
                <h2 className="text-2xl sm:text-3xl font-bold tracking-widest text-stone-900 font-serif">
                  畇朵報價單
                </h2>
              </div>

              {/* Client & Metadata Info Grid */}
              <div className="grid grid-cols-1 sm:grid-cols-2 gap-x-6 gap-y-2 mb-4 text-xs sm:text-sm border border-stone-900 p-3 sm:p-4 rounded-sm bg-stone-50/50 print:bg-transparent">
                <div className="flex items-center">
                  <span className="font-bold text-stone-900 min-w-[70px]">案名：</span>
                  <input
                    type="text"
                    value={quoteData.projectName || ''}
                    onChange={(e) => setQuoteData({ ...quoteData, projectName: e.target.value })}
                    placeholder="請輸入客戶或案名"
                    className="flex-1 bg-transparent border-b border-dashed border-stone-300 focus:border-amber-700 outline-none px-1 py-0.5 print:border-none font-bold text-stone-900"
                  />
                </div>

                <div className="flex items-center">
                  <span className="font-bold text-stone-900 min-w-[80px]">報價日期：</span>
                  <input
                    type="text"
                    value={quoteData.quoteDate || ''}
                    onChange={(e) => setQuoteData({ ...quoteData, quoteDate: e.target.value })}
                    placeholder="115/11/06"
                    className="flex-1 bg-transparent border-b border-dashed border-stone-300 focus:border-amber-700 outline-none px-1 py-0.5 print:border-none"
                  />
                </div>

                <div className="flex items-center">
                  <span className="font-bold text-stone-900 min-w-[70px]">聯絡電話：</span>
                  <input
                    type="text"
                    value={quoteData.phone || ''}
                    onChange={(e) => setQuoteData({ ...quoteData, phone: e.target.value })}
                    placeholder="請輸入電話"
                    className="flex-1 bg-transparent border-b border-dashed border-stone-300 focus:border-amber-700 outline-none px-1 py-0.5 print:border-none"
                  />
                </div>

                <div className="flex items-center">
                  <span className="font-bold text-stone-900 min-w-[80px]">付款方式：</span>
                  <input
                    type="text"
                    value={quoteData.paymentMethod || ''}
                    onChange={(e) => setQuoteData({ ...quoteData, paymentMethod: e.target.value })}
                    placeholder="現金、匯款或即期支票"
                    className="flex-1 bg-transparent border-b border-dashed border-stone-300 focus:border-amber-700 outline-none px-1 py-0.5 print:border-none"
                  />
                </div>

                <div className="flex items-center sm:col-span-1">
                  <span className="font-bold text-stone-900 min-w-[70px]">地址：</span>
                  <input
                    type="text"
                    value={quoteData.address || ''}
                    onChange={(e) => setQuoteData({ ...quoteData, address: e.target.value })}
                    placeholder="請輸入施工地址"
                    className="flex-1 bg-transparent border-b border-dashed border-stone-300 focus:border-amber-700 outline-none px-1 py-0.5 print:border-none"
                  />
                </div>

                <div className="flex items-center sm:col-span-1">
                  <span className="font-bold text-stone-900 min-w-[80px]">承辦業務：</span>
                  <input
                    type="text"
                    value={quoteData.salesRep || ''}
                    onChange={(e) => setQuoteData({ ...quoteData, salesRep: e.target.value })}
                    placeholder="業務姓名 (例如: Bella)"
                    className="flex-1 bg-transparent border-b border-dashed border-stone-300 focus:border-amber-700 outline-none px-1 py-0.5 print:border-none font-semibold text-stone-800"
                  />
                </div>
              </div>

              {/* Items Table */}
              <div className="overflow-x-auto mb-4">
                <table className="w-full border-collapse border border-stone-900 text-xs sm:text-sm">
                  <thead>
                    <tr className="bg-[#c8b284] text-stone-900 font-bold border-b border-stone-900">
                      <th className="border border-stone-900 p-2 text-center w-[18%]">項目</th>
                      <th className="border border-stone-900 p-2 text-center w-[40%]">品名</th>
                      <th className="border border-stone-900 p-2 text-center w-[12%]">數量</th>
                      <th className="border border-stone-900 p-2 text-center w-[15%]">單價</th>
                      <th className="border border-stone-900 p-2 text-center w-[15%]">金額</th>
                      <th className="border border-stone-900 p-1 text-center w-[5%] print:hidden no-print"></th>
                    </tr>
                  </thead>
                  <tbody>
                    {(quoteData.items || []).map((item, index) => {
                      const qtyNum = parseFloat(item.qty);
                      const priceNum = parseFloat(item.price);
                      const hasCalc = !isNaN(qtyNum) && !isNaN(priceNum);
                      const rowSubtotal = hasCalc ? qtyNum * priceNum : null;

                      return (
                        <tr key={item.id || index} className="hover:bg-amber-50/30 transition border-b border-stone-900">
                          {/* Category */}
                          <td className="border border-stone-900 p-1.5 text-center">
                            <input
                              type="text"
                              value={item.category || ''}
                              onChange={(e) => handleItemChange(index, 'category', e.target.value)}
                              placeholder=""
                              className="w-full bg-transparent text-center focus:outline-none focus:bg-amber-100/50 print:border-none"
                            />
                          </td>

                          {/* Description */}
                          <td className="border border-stone-900 p-1.5">
                            <input
                              type="text"
                              value={item.name || ''}
                              onChange={(e) => handleItemChange(index, 'name', e.target.value)}
                              placeholder=""
                              className="w-full bg-transparent text-left focus:outline-none focus:bg-amber-100/50 print:border-none"
                            />
                          </td>

                          {/* Qty */}
                          <td className="border border-stone-900 p-1.5 text-center">
                            <input
                              type="text"
                              value={item.qty}
                              onChange={(e) => handleItemChange(index, 'qty', e.target.value)}
                              className="w-full bg-transparent text-center focus:outline-none focus:bg-amber-100/50 print:border-none"
                            />
                          </td>

                          {/* Price */}
                          <td className="border border-stone-900 p-1.5 text-right font-mono">
                            <input
                              type="text"
                              value={item.price}
                              onChange={(e) => handleItemChange(index, 'price', e.target.value)}
                              className="w-full bg-transparent text-right focus:outline-none focus:bg-amber-100/50 print:border-none"
                            />
                          </td>

                          {/* Amount */}
                          <td className="border border-stone-900 p-1.5 text-right font-mono font-medium">
                            {rowSubtotal !== null ? rowSubtotal.toLocaleString() : ''}
                          </td>

                          {/* Remove button */}
                          <td className="border border-stone-900 p-1 text-center print:hidden no-print">
                            <button
                              onClick={() => removeItemRow(index)}
                              className="text-stone-400 hover:text-red-600 transition p-1"
                              title="刪除列"
                            >
                              <Trash2 className="w-3.5 h-3.5" />
                            </button>
                          </td>
                        </tr>
                      );
                    })}
                  </tbody>
                </table>
              </div>

              {/* Add Row Button */}
              <div className="mb-4 print:hidden no-print">
                <button
                  onClick={addItemRow}
                  className="w-full py-2 border-2 border-dashed border-stone-300 rounded-lg text-stone-600 hover:border-amber-700 hover:text-amber-800 transition text-xs sm:text-sm font-medium flex items-center justify-center space-x-1"
                >
                  <Plus className="w-4 h-4" />
                  <span>新增品項 / 備註欄位</span>
                </button>
              </div>

              {/* Terms & Totals */}
              <div className="grid grid-cols-1 sm:grid-cols-12 gap-0 border border-stone-900 mb-3 text-xs sm:text-sm">
                
                {/* Terms Section */}
                <div className="sm:col-span-7 p-2.5 border-b sm:border-b-0 sm:border-r border-stone-900 bg-stone-50/30">
                  <p className="font-bold text-stone-900 mb-1">為確保雙方權益及服務品質，相關合作條款如下：</p>
                  <ol className="list-decimal list-inside space-y-0.5 text-stone-700 leading-tight text-[11px] sm:text-xs">
                    {TERMS_TEXT.map((term, i) => (
                      <li key={i}>{term}</li>
                    ))}
                  </ol>
                </div>

                {/* Sum & Custom Discount Total */}
                <div className="sm:col-span-5 flex flex-col justify-between">
                  <div className="flex border-b border-stone-900 h-1/2">
                    <div className="w-1/3 bg-[#c8b284] p-2 font-bold text-stone-900 text-center flex items-center justify-center border-r border-stone-900">
                      總計
                    </div>
                    <div className="w-2/3 p-2 text-right font-mono font-bold text-base text-stone-900 flex items-center justify-end">
                      ${rawTotal.toLocaleString()}
                    </div>
                  </div>

                  <div className="flex h-1/2">
                    <div className="w-1/3 bg-[#c8b284] p-2 font-bold text-stone-900 text-center flex items-center justify-center border-r border-stone-900">
                      優惠價含稅
                    </div>
                    <div className="w-2/3 p-2 text-right font-mono font-bold text-lg text-amber-900 flex items-center justify-end bg-amber-50/40">
                      <span className="text-xs mr-1 text-stone-500 font-normal print:hidden no-print">$</span>
                      <input
                        type="number"
                        value={quoteData.customDiscountedTotal}
                        onChange={(e) => setQuoteData({ ...quoteData, customDiscountedTotal: e.target.value })}
                        className="w-full text-right bg-transparent border-b border-dashed border-amber-400 focus:outline-none font-bold print:border-none"
                        placeholder="請輸入優惠總價"
                      />
                    </div>
                  </div>
                </div>
              </div>

              {/* Deposit Banner */}
              <div className="flex items-center justify-between bg-amber-50/60 px-3 py-2 rounded border border-amber-200 text-xs mb-4 print:hidden no-print">
                <span className="text-stone-600 font-medium flex items-center">
                  <CreditCard className="w-3.5 h-3.5 mr-1.5 text-amber-800" />
                  試算 50% 訂金金額：
                </span>
                <span className="font-mono font-bold text-amber-900 text-sm">
                  ${depositAmount.toLocaleString()} 元
                </span>
              </div>

              {/* Bank Transfer Info & Signatures */}
              <div className="pt-1">
                <div className="text-[11px] text-stone-700 mb-5 bg-stone-50 p-1.5 border border-stone-300 text-center sm:text-left">
                  <span className="font-bold text-stone-900">【匯款資訊】</span> {BANK_INFO}
                </div>

                <div className="grid grid-cols-2 gap-8 text-center pt-2">
                  <div className="flex flex-col items-center">
                    <div className="h-10 flex items-end justify-center font-serif text-base text-stone-800 font-semibold border-b border-stone-900 w-3/4 pb-0.5">
                      {quoteData.salesSignature || quoteData.salesRep}
                    </div>
                    <span className="text-xs font-bold text-stone-800 mt-1">公司業務簽章</span>
                  </div>

                  <div className="flex flex-col items-center">
                    <div className="h-10 border-b border-stone-900 w-3/4"></div>
                    <span className="text-xs font-bold text-stone-800 mt-1">客戶簽章</span>
                  </div>
                </div>
              </div>

            </div>
          </div>
        )}

        {/* --- DASHBOARD TAB --- */}
        {activeTab === 'dashboard' && (
          <div className="space-y-6">
            {/* ... Dashboard content remains identical ... */}
            <div className="bg-white rounded-xl shadow-sm border border-stone-200 p-4 flex flex-col sm:flex-row items-center justify-between gap-4">
              <div className="relative w-full sm:w-80">
                <Search className="w-4 h-4 absolute left-3 top-1/2 transform -translate-y-1/2 text-stone-400" />
                <input
                  type="text"
                  placeholder="搜尋案名、業務、電話、地址..."
                  value={searchTerm}
                  onChange={(e) => setSearchTerm(e.target.value)}
                  className="w-full pl-9 pr-4 py-2 bg-stone-50 border border-stone-200 rounded-lg text-sm focus:outline-none focus:ring-2 focus:ring-amber-700/50"
                />
              </div>

              <div className="flex items-center space-x-3 w-full sm:w-auto justify-end">
                <button
                  onClick={handleExportCSV}
                  className="px-4 py-2 bg-emerald-700 hover:bg-emerald-800 text-white rounded-lg text-xs sm:text-sm font-medium transition flex items-center space-x-1.5 shadow-sm"
                >
                  <FileSpreadsheet className="w-4 h-4" />
                  <span>匯出 Excel / CSV 總表</span>
                </button>

                <button
                  onClick={handleCreateNew}
                  className="px-4 py-2 bg-amber-800 hover:bg-amber-900 text-white rounded-lg text-xs sm:text-sm font-medium transition flex items-center space-x-1.5 shadow-sm"
                >
                  <Plus className="w-4 h-4" />
                  <span>開新報價單</span>
                </button>
              </div>
            </div>

            <div className="bg-white rounded-xl shadow-sm border border-stone-200 overflow-hidden">
              <div className="px-6 py-4 border-b border-stone-100 flex items-center justify-between">
                <h3 className="font-bold text-stone-800 text-base">歷史報價與客戶紀錄</h3>
                <span className="text-xs text-stone-500">共 {filteredQuotes.length} 筆紀錄</span>
              </div>

              {loading ? (
                <div className="p-12 text-center text-stone-400 flex flex-col items-center">
                  <RefreshCw className="w-8 h-8 animate-spin mb-2 text-amber-700" />
                  <span>資料讀取中...</span>
                </div>
              ) : filteredQuotes.length === 0 ? (
                <div className="p-12 text-center text-stone-500">
                  <FileText className="w-12 h-12 mx-auto mb-3 text-stone-300" />
                  <p className="font-medium text-base">尚無搜尋結果或歷史報價單</p>
                  <p className="text-xs text-stone-400 mt-1">您可以點擊「開新報價單」進行製作與存檔。</p>
                </div>
              ) : (
                <div className="overflow-x-auto">
                  <table className="w-full text-left text-sm">
                    <thead className="bg-stone-50 text-stone-600 font-semibold border-b border-stone-200 text-xs">
                      <tr>
                        <th className="px-6 py-3">案名</th>
                        <th className="px-6 py-3">承辦業務</th>
                        <th className="px-6 py-3">報價日期</th>
                        <th className="px-6 py-3">電話 / 地址</th>
                        <th className="px-6 py-3 text-right">試算金額</th>
                        <th className="px-6 py-3 text-right">優惠成交價</th>
                        <th className="px-6 py-3 text-center">操作</th>
                      </tr>
                    </thead>
                    <tbody className="divide-y divide-stone-100 text-stone-700">
                      {filteredQuotes.map((q) => (
                        <tr 
                          key={q.id} 
                          onClick={() => handleLoadQuote(q)}
                          className="hover:bg-amber-50/40 transition cursor-pointer"
                        >
                          <td className="px-6 py-4 font-bold text-stone-900">
                            {q.projectName || '未命名案名'}
                          </td>
                          <td className="px-6 py-4">
                            <span className="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-amber-100 text-amber-900">
                              {q.salesRep || '無'}
                            </span>
                          </td>
                          <td className="px-6 py-4 text-xs font-mono text-stone-500">
                            {q.quoteDate || '-'}
                          </td>
                          <td className="px-6 py-4 text-xs">
                            <div className="font-medium text-stone-800">{q.phone || '-'}</div>
                            <div className="text-stone-400 truncate max-w-xs">{q.address || '-'}</div>
                          </td>
                          <td className="px-6 py-4 text-right font-mono text-xs text-stone-500">
                            ${(q.rawTotal || 0).toLocaleString()}
                          </td>
                          <td className="px-6 py-4 text-right font-mono font-bold text-amber-900">
                            ${(q.finalPrice || 0).toLocaleString()}
                          </td>
                          <td className="px-6 py-4 text-center">
                            <div className="flex items-center justify-center space-x-2">
                              <button
                                onClick={(e) => {
                                  e.stopPropagation();
                                  handleLoadQuote(q);
                                }}
                                className="p-1.5 text-stone-600 hover:text-amber-800 hover:bg-amber-100 rounded transition"
                                title="載入編輯與列印"
                              >
                                <Edit3 className="w-4 h-4" />
                              </button>
                              <button
                                onClick={(e) => handleDeleteQuote(q.id, e)}
                                className="p-1.5 text-stone-400 hover:text-red-600 hover:bg-red-50 rounded transition"
                                title="刪除"
                              >
                                <Trash2 className="w-4 h-4" />
                              </button>
                            </div>
                          </td>
                        </tr>
                      ))}
                    </tbody>
                  </table>
                </div>
              )}
            </div>

          </div>
        )}

      </main>

      {/* --- 🌟 FULLSCREEN PRINT PREVIEW MODAL 🌟 --- */}
      {showPrintModal && (
        <div className="fixed inset-0 z-50 bg-stone-900/80 backdrop-blur-sm flex flex-col justify-between overflow-y-auto print:p-0 print:bg-white print:static">
          
          {/* Top Modal Controls (Hidden during actual print) */}
          <div className="bg-stone-900 text-white p-4 flex items-center justify-between border-b border-stone-800 sticky top-0 z-50 print:hidden">
            <div className="flex items-center space-x-2">
              <Printer className="w-5 h-5 text-amber-400" />
              <span className="font-semibold text-base">A4 報價單匯出與列印中心</span>
            </div>

            <div className="flex items-center space-x-3">
              <button
                onClick={handleSystemPrint}
                className="px-4 py-2 bg-amber-700 hover:bg-amber-800 text-white rounded-lg text-sm font-medium transition flex items-center space-x-1.5 shadow"
              >
                <Printer className="w-4 h-4" />
                <span>直接列印 / 另存 PDF</span>
              </button>

              <button
                onClick={handleDownloadStandaloneHTML}
                className="px-4 py-2 bg-emerald-700 hover:bg-emerald-800 text-white rounded-lg text-sm font-medium transition flex items-center space-x-1.5 shadow"
              >
                <Download className="w-4 h-4" />
                <span>一鍵下載獨立列印檔 (.html)</span>
              </button>

              <button
                onClick={() => setShowPrintModal(false)}
                className="p-2 text-stone-400 hover:text-white rounded-lg hover:bg-stone-800 transition"
              >
                <X className="w-5 h-5" />
              </button>
            </div>
          </div>

          {/* Printable Container */}
          <div className="p-4 sm:p-10 flex-1 flex justify-center items-start print:p-0">
            <div className="bg-white text-stone-900 shadow-2xl rounded-sm p-8 max-w-[210mm] w-full mx-auto print:shadow-none print:p-0">
              
              {/* Header */}
              <div className="text-center pb-2 mb-4 border-b-2 border-stone-900">
                <h2 className="text-2xl sm:text-3xl font-bold tracking-widest text-stone-900 font-serif">
                  畇朵報價單
                </h2>
              </div>

              {/* Info Table */}
              <div className="grid grid-cols-2 gap-x-6 gap-y-2 mb-4 text-xs sm:text-sm border border-stone-900 p-3">
                <div><span className="font-bold">案名：</span>{quoteData.projectName}</div>
                <div><span className="font-bold">報價日期：</span>{quoteData.quoteDate}</div>
                <div><span className="font-bold">聯絡電話：</span>{quoteData.phone}</div>
                <div><span className="font-bold">付款方式：</span>{quoteData.paymentMethod}</div>
                <div><span className="font-bold">地址：</span>{quoteData.address}</div>
                <div><span className="font-bold">承辦業務：</span>{quoteData.salesRep}</div>
              </div>

              {/* Items Table */}
              <table className="w-full border-collapse border border-stone-900 text-xs sm:text-sm mb-4">
                <thead>
                  <tr className="bg-[#c8b284] text-stone-900 font-bold border-b border-stone-900">
                    <th className="border border-stone-900 p-2 text-center w-[18%]">項目</th>
                    <th className="border border-stone-900 p-2 text-center w-[40%]">品名</th>
                    <th className="border border-stone-900 p-2 text-center w-[12%]">數量</th>
                    <th className="border border-stone-900 p-2 text-center w-[15%]">單價</th>
                    <th className="border border-stone-900 p-2 text-center w-[15%]">金額</th>
                  </tr>
                </thead>
                <tbody>
                  {(quoteData.items || []).map((item, idx) => {
                    const q = parseFloat(item.qty);
                    const p = parseFloat(item.price);
                    const sub = (!isNaN(q) && !isNaN(p)) ? (q * p).toLocaleString() : '';
                    return (
                      <tr key={idx} className="border-b border-stone-900">
                        <td className="border border-stone-900 p-2 text-center">{item.category}</td>
                        <td className="border border-stone-900 p-2 text-left">{item.name}</td>
                        <td className="border border-stone-900 p-2 text-center">{item.qty}</td>
                        <td className="border border-stone-900 p-2 text-right font-mono">{item.price}</td>
                        <td className="border border-stone-900 p-2 text-right font-mono font-bold">{sub}</td>
                      </tr>
                    );
                  })}
                </tbody>
              </table>

              {/* Terms & Totals */}
              <div className="grid grid-cols-12 gap-0 border border-stone-900 mb-4 text-xs sm:text-sm">
                <div className="col-span-7 p-2.5 border-r border-stone-900">
                  <p className="font-bold text-stone-900 mb-1">為確保雙方權益及服務品質，相關合作條款如下：</p>
                  <ol className="list-decimal list-inside space-y-0.5 text-stone-700 text-[11px]">
                    {TERMS_TEXT.map((t, i) => <li key={i}>{t}</li>)}
                  </ol>
                </div>

                <div className="col-span-5 flex flex-col justify-between">
                  <div className="flex border-b border-stone-900 h-1/2">
                    <div className="w-1/3 bg-[#c8b284] p-2 font-bold text-stone-900 text-center flex items-center justify-center border-r border-stone-900">
                      總計
                    </div>
                    <div className="w-2/3 p-2 text-right font-mono font-bold text-base text-stone-900 flex items-center justify-end">
                      ${rawTotal.toLocaleString()}
                    </div>
                  </div>

                  <div className="flex h-1/2">
                    <div className="w-1/3 bg-[#c8b284] p-2 font-bold text-stone-900 text-center flex items-center justify-center border-r border-stone-900">
                      優惠價含稅
                    </div>
                    <div className="w-2/3 p-2 text-right font-mono font-bold text-lg text-amber-900 flex items-center justify-end">
                      ${finalPrice.toLocaleString()}
                    </div>
                  </div>
                </div>
              </div>

              {/* Bank & Signatures */}
              <div className="text-[11px] text-stone-700 mb-6 bg-stone-50 p-2 border border-stone-300 text-center">
                <span className="font-bold text-stone-900">【匯款資訊】</span> {BANK_INFO}
              </div>

              <div className="grid grid-cols-2 gap-8 text-center pt-2">
                <div className="flex flex-col items-center">
                  <div className="h-10 flex items-end justify-center font-serif text-base text-stone-800 font-semibold border-b border-stone-900 w-3/4 pb-0.5">
                    {quoteData.salesSignature || quoteData.salesRep}
                  </div>
                  <span className="text-xs font-bold text-stone-800 mt-1">公司業務簽章</span>
                </div>

                <div className="flex flex-col items-center">
                  <div className="h-10 border-b border-stone-900 w-3/4"></div>
                  <span className="text-xs font-bold text-stone-800 mt-1">客戶簽章</span>
                </div>
              </div>

            </div>
          </div>
        </div>
      )}

      {/* Print-specific CSS Overrides */}
      <style>{`
        @media print {
          @page {
            size: A4 portrait;
            margin: 8mm;
          }
          body {
            background-color: white !important;
            color: black !important;
            -webkit-print-color-adjust: exact !important;
            print-color-adjust: exact !important;
          }
          .print\\:hidden, .no-print {
            display: none !important;
          }
          input {
            border: none !important;
            background: transparent !important;
            box-shadow: none !important;
          }
        }
      `}</style>
    </div>
  );
}
