<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<title>أداة إدخال وتحليل نتائج الطلاب</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
/* إعادة تعيين عام */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    -webkit-tap-highlight-color: transparent;
}

/* تحسينات للهواتف */
html {
    font-size: 14px;
    -webkit-text-size-adjust: 100%;
    -moz-text-size-adjust: 100%;
    -ms-text-size-adjust: 100%;
}

body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Tahoma, Arial, sans-serif;
    background: #f5f7fa;
    color: #333;
    line-height: 1.5;
    padding: 10px;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
}

/* تحسينات للأجهزة المختلفة */
@media (max-width: 480px) {
    html { font-size: 12px; }
    .container { padding: 0 5px; }
}

@media (min-width: 481px) and (max-width: 768px) {
    html { font-size: 13px; }
}

@media (min-width: 769px) and (max-width: 1024px) {
    html { font-size: 15px; }
}

@media (min-width: 1025px) {
    html { font-size: 16px; }
    .container { max-width: 1200px; }
}

/* تحسينات للشاشات الكبيرة */
@media (min-width: 1400px) {
    .container { max-width: 1400px; }
}

/* تحسينات خاصة لآيفون */
@supports (-webkit-touch-callout: none) {
    body {
        padding-bottom: env(safe-area-inset-bottom);
    }
    .fixed-bottom {
        padding-bottom: env(safe-area-inset-bottom);
    }
}

.container {
    width: 100%;
    margin: 0 auto;
    flex: 1;
}

/* تحسين التبويبات للأجهزة المحمولة */
.tabs {
    display: flex;
    margin-bottom: 15px;
    border-bottom: 2px solid #ddd;
    overflow-x: auto;
    -webkit-overflow-scrolling: touch;
    scrollbar-width: none; /* Firefox */
}

.tabs::-webkit-scrollbar {
    display: none; /* Chrome, Safari, Opera */
}

.tab {
    padding: 12px 20px;
    cursor: pointer;
    font-weight: bold;
    border-radius: 8px 8px 0 0;
    background: #e9ecef;
    margin-left: 5px;
    transition: all 0.3s;
    white-space: nowrap;
    flex-shrink: 0;
    display: flex;
    align-items: center;
    gap: 8px;
}

.tab i {
    font-size: 1.1rem;
}

.tab.active {
    background: #1a5c9e;
    color: white;
}

.tab:hover {
    background: #d0d7e0;
}

.tab.active:hover {
    background: #1a5c9e;
}

.tab-content {
    display: none;
    background: white;
    padding: 15px;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    margin-bottom: 20px;
    animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
}

.tab-content.active {
    display: block;
}

/* تحسين مربعات الإدخال للأجهزة المحمولة */
.input-section {
    background: #f8f9fa;
    padding: 15px;
    border-radius: 10px;
    margin-bottom: 15px;
}

.input-section h2 {
    margin-bottom: 15px;
    color: #1a5c9e;
    font-size: 1.3rem;
}

.input-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 12px;
    margin-bottom: 15px;
}

@media (max-width: 480px) {
    .input-grid {
        grid-template-columns: 1fr;
    }
}

.input-group {
    display: flex;
    flex-direction: column;
}

.input-group label {
    margin-bottom: 6px;
    font-weight: bold;
    color: #555;
    font-size: 0.95rem;
}

.input-group input, .input-group select {
    padding: 12px;
    border: 1px solid #ddd;
    border-radius: 8px;
    font-size: 1rem;
    background: white;
    transition: border-color 0.3s;
    width: 100%;
}

.input-group input:focus, .input-group select:focus {
    border-color: #1a5c9e;
    outline: none;
    box-shadow: 0 0 0 2px rgba(26, 92, 158, 0.1);
}

/* تحسين الأزرار للأجهزة المحمولة */
.actions {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    margin-top: 20px;
}

button {
    background: #25d366;
    color: #fff;
    border: none;
    padding: 14px 24px;
    font-size: 1rem;
    border-radius: 8px;
    cursor: pointer;
    font-weight: bold;
    transition: all 0.3s;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    flex: 1;
    min-width: 140px;
    min-height: 50px;
}

button:hover {
    background: #1da851;
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

button:active {
    transform: translateY(0);
}

button.secondary {
    background: #6c757d;
}

button.secondary:hover {
    background: #5a6268;
}

button.danger {
    background: #dc3545;
}

button.danger:hover {
    background: #c82333;
}

button.whatsapp {
    background: #25d366;
}

button.whatsapp:hover {
    background: #1da851;
}

button i {
    font-size: 1.1rem;
}

/* تحسين الجداول للأجهزة المحمولة */
.students-table-container {
    overflow-x: auto;
    -webkit-overflow-scrolling: touch;
    margin-top: 15px;
    border-radius: 8px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.students-table {
    width: 100%;
    border-collapse: collapse;
    background: white;
    min-width: 600px;
}

.students-table th {
    background: #1a5c9e;
    color: white;
    padding: 14px;
    text-align: center;
    font-weight: bold;
    font-size: 0.95rem;
}

.students-table td {
    padding: 12px;
    text-align: center;
    border-bottom: 1px solid #eee;
    font-size: 0.95rem;
}

.students-table tr:hover {
    background: #f8f9fa;
}

.delete-btn {
    background: #dc3545;
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 6px;
    cursor: pointer;
    font-size: 0.9rem;
    display: flex;
    align-items: center;
    gap: 5px;
    margin: 0 auto;
}

.delete-btn:hover {
    background: #c82333;
}

/* تحسين بطاقات الملخص */
.summary-cards {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 12px;
    margin-bottom: 20px;
}

@media (max-width: 480px) {
    .summary-cards {
        grid-template-columns: 1fr;
    }
}

.summary-card {
    background: white;
    padding: 15px;
    border-radius: 10px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    text-align: center;
    transition: transform 0.3s;
}

.summary-card:hover {
    transform: translateY(-3px);
}

.summary-card h3 {
    margin-top: 0;
    color: #1a5c9e;
    font-size: 1rem;
    margin-bottom: 10px;
}

.summary-card .value {
    font-size: 1.8rem;
    font-weight: bold;
    margin: 10px 0;
    color: #333;
}

.summary-card .subtext {
    font-size: 0.9rem;
    color: #666;
}

/* تحسين الرسوم البيانية */
.charts-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 15px;
    margin-bottom: 20px;
}

@media (max-width: 768px) {
    .charts-container {
        grid-template-columns: 1fr;
    }
}

.chart-box {
    background: white;
    border: 1px solid #ddd;
    border-radius: 10px;
    padding: 15px;
    height: 280px;
    display: flex;
    flex-direction: column;
}

.chart-box h3 {
    margin: 0 0 10px 0;
    color: #1a5c9e;
    font-size: 1.1rem;
    text-align: center;
}

.chart-box canvas {
    flex: 1;
    width: 100% !important;
    height: 100% !important;
}

/* تحسين صفحة التقرير */
.page {
    background: #fff;
    width: 210mm;
    min-height: 297mm;
    margin: 20px auto;
    padding: 20mm;
    box-sizing: border-box;
    box-shadow: 0 5px 15px rgba(0,0,0,0.2);
}

@media print {
    .page {
        margin: 0;
        padding: 0;
        box-shadow: none;
    }
}

.header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 0.9rem;
    margin-bottom: 15px;
    padding-bottom: 10px;
    border-bottom: 1px solid #eee;
}

.title {
    text-align: center;
    font-size: 1.5rem;
    font-weight: bold;
    margin: 20px 0;
    padding: 15px;
    background: #1a5c9e;
    color: #fff;
    border-radius: 10px;
}

.info {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 10px;
    margin-bottom: 15px;
}

.box {
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 12px;
    text-align: center;
    font-size: 0.95rem;
}

.box span {
    display: block;
    font-weight: bold;
    margin-top: 5px;
    font-size: 1.1rem;
    color: #1a5c9e;
}

.summary {
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 15px;
    font-size: 0.95rem;
    margin-bottom: 20px;
    line-height: 1.6;
}

.summary strong {
    color: #1a5c9e;
}

.section-title {
    font-size: 1.2rem;
    font-weight: bold;
    margin: 20px 0 10px;
    color: #1a5c9e;
    padding-bottom: 5px;
    border-bottom: 2px solid #eee;
}

.table {
    border: 1px solid #ddd;
    border-radius: 8px;
    overflow: hidden;
    margin-bottom: 15px;
}

.row {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    border-bottom: 1px solid #eee;
}

.cell {
    padding: 10px;
    text-align: center;
    font-size: 0.95rem;
}

.level {
    color: #fff;
    font-weight: bold;
    padding: 5px 12px;
    border-radius: 6px;
    font-size: 0.9rem;
    display: inline-block;
    min-width: 80px;
}

.excellent { background: linear-gradient(135deg, #4caf50, #2e7d32); }
.verygood { background: linear-gradient(135deg, #009688, #00695c); }
.good { background: linear-gradient(135deg, #2196f3, #0d47a1); }
.pass { background: linear-gradient(135deg, #ff9800, #ef6c00); }
.weak { background: linear-gradient(135deg, #f44336, #c62828); }

.stats {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 10px;
    margin-bottom: 20px;
}

.stat {
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 15px;
    text-align: center;
    font-size: 0.95rem;
}

.stat strong {
    display: block;
    font-size: 1.5rem;
    margin-top: 5px;
    color: #1a5c9e;
}

.footer {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 0.9rem;
    margin-top: 30px;
    padding-top: 20px;
    border-top: 1px solid #eee;
}

.hidden {
    display: none !important;
}

/* إدارة الصفوف */
.grade-management {
    background: white;
    padding: 15px;
    border-radius: 10px;
    margin-top: 20px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.grade-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 15px;
}

.grade-inputs {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 10px;
    margin-bottom: 15px;
}

.grade-list {
    background: #f8f9fa;
    padding: 15px;
    border-radius: 8px;
    margin-top: 15px;
}

.grade-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
    border-bottom: 1px solid #ddd;
}

.grade-item:last-child {
    border-bottom: none;
}

/* رسائل التنبيه */
.alert {
    padding: 12px 15px;
    border-radius: 8px;
    margin: 10px 0;
    display: flex;
    align-items: center;
    gap: 10px;
    animation: slideIn 0.3s ease;
}

@keyframes slideIn {
    from { transform: translateX(-20px); opacity: 0; }
    to { transform: translateX(0); opacity: 1; }
}

.alert.success {
    background: #d4edda;
    color: #155724;
    border: 1px solid #c3e6cb;
}

.alert.warning {
    background: #fff3cd;
    color: #856404;
    border: 1px solid #ffeaa7;
}

.alert.error {
    background: #f8d7da;
    color: #721c24;
    border: 1px solid #f5c6cb;
}

/* أزرار التحميل */
.download-buttons {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    margin: 20px 0;
}

/* الشعارات */
.logo {
    text-align: center;
    margin-bottom: 20px;
}

.logo img {
    max-width: 120px;
    height: auto;
}

/* تحسينات للطباعة */
@media print {
    body * {
        visibility: hidden;
    }
    
    .page, .page * {
        visibility: visible;
    }
    
    .page {
        position: absolute;
        left: 0;
        top: 0;
        width: 210mm;
        min-height: 297mm;
        margin: 0;
        padding: 20mm;
        box-shadow: none;
        page-break-after: always;
    }
    
    .no-print {
        display: none !important;
    }
}

/* تحسينات للوضع الأفقي على الهواتف */
@media (max-height: 500px) and (orientation: landscape) {
    .input-grid {
        grid-template-columns: repeat(2, 1fr);
    }
    
    .charts-container {
        grid-template-columns: repeat(2, 1fr);
    }
}

/* تحسينات للنصوص الطويلة */
.truncate {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    max-width: 150px;
}
</style>
</head>

<body>

<div class="container">
    <!-- الشعار -->
    <div class="logo">
        <h1 style="color: #1a5c9e; margin-bottom: 5px;">📊 أداة تحليل نتائج الطلاب</h1>
        <p style="color: #666; font-size: 0.9rem;">أداة متكاملة لإدخال وتحليل نتائج جميع المواد الدراسية</p>
    </div>
    
    <!-- التبويبات -->
    <div class="tabs">
        <div class="tab active" onclick="switchTab('input')">
            <i class="fas fa-user-plus"></i>
            <span>إدخال البيانات</span>
        </div>
        <div class="tab" onclick="switchTab('grades')">
            <i class="fas fa-school"></i>
            <span>إدارة الصفوف</span>
        </div>
        <div class="tab" onclick="switchTab('analysis')">
            <i class="fas fa-chart-bar"></i>
            <span>تحليل النتائج</span>
        </div>
        <div class="tab" onclick="switchTab('report')">
            <i class="fas fa-file-pdf"></i>
            <span>تقرير PDF</span>
        </div>
    </div>
    
    <!-- تبويب إدخال البيانات -->
    <div id="input-tab" class="tab-content active">
        <div class="input-section">
            <h2><i class="fas fa-user-plus"></i> إضافة طالب جديد</h2>
            
            <div id="alert-message" class="alert hidden"></div>
            
            <div class="input-grid">
                <div class="input-group">
                    <label><i class="fas fa-user"></i> اسم الطالب</label>
                    <input type="text" id="studentName" placeholder="أدخل اسم الطالب الكامل">
                </div>
                <div class="input-group">
                    <label><i class="fas fa-book"></i> المادة</label>
                    <select id="subject">
                        <option value="الرياضيات">الرياضيات</option>
                        <option value="اللغة العربية">اللغة العربية</option>
                        <option value="اللغة الإنجليزية">اللغة الإنجليزية</option>
                        <option value="العلوم">العلوم</option>
                        <option value="الدراسات الاجتماعية">الدراسات الاجتماعية</option>
                        <option value="التربية الإسلامية">التربية الإسلامية</option>
                        <option value="الحاسب الآلي">الحاسب الآلي</option>
                        <option value="الفنية">الفنية</option>
                        <option value="التربية البدنية">التربية البدنية</option>
                    </select>
                </div>
                <div class="input-group">
                    <label><i class="fas fa-graduation-cap"></i> الفصل</label>
                    <select id="className" style="appearance: none; -webkit-appearance: none;">
                        <!-- سيتم تعبئتها ديناميكياً -->
                    </select>
                </div>
                <div class="input-group">
                    <label><i class="fas fa-star"></i> الدرجة (من 40)</label>
                    <input type="number" id="score" min="0" max="40" step="0.5" placeholder="أدخل الدرجة من 40">
                </div>
            </div>
            
            <div class="actions">
                <button onclick="addStudent()" class="success">
                    <i class="fas fa-plus-circle"></i>
                    <span>إضافة الطالب</span>
                </button>
                <button onclick="clearForm()" class="secondary">
                    <i class="fas fa-broom"></i>
                    <span>تفريغ الحقول</span>
                </button>
                <button onclick="generateSampleData()" class="secondary">
                    <i class="fas fa-flask"></i>
                    <span>بيانات تجريبية</span>
                </button>
            </div>
        </div>
        
        <h3><i class="fas fa-users"></i> قائمة الطلاب المدخلين</h3>
        <div class="students-table-container">
            <table class="students-table" id="studentsList">
                <thead>
                    <tr>
                        <th>#</th>
                        <th>اسم الطالب</th>
                        <th>المادة</th>
                        <th>الفصل</th>
                        <th>الدرجة</th>
                        <th>المستوى</th>
                        <th>الإجراءات</th>
                    </tr>
                </thead>
                <tbody id="studentsTableBody">
                    <!-- سيتم إضافة الطلاب هنا -->
                </tbody>
            </table>
        </div>
    </div>
    
    <!-- تبويب إدارة الصفوف -->
    <div id="grades-tab" class="tab-content">
        <div class="grade-management">
            <h2><i class="fas fa-school"></i> إدارة الصفوف الدراسية</h2>
            
            <div id="grade-alert" class="alert hidden"></div>
            
            <div class="grade-header">
                <h3>إضافة صف جديد</h3>
                <button onclick="resetGrades()" class="secondary">
                    <i class="fas fa-undo"></i>
                    <span>إعادة التعيين</span>
                </button>
            </div>
            
            <div class="grade-inputs">
                <div class="input-group">
                    <label>المرحلة الدراسية</label>
                    <input type="text" id="gradeStage" placeholder="مثال: متوسط" value="متوسط">
                </div>
                <div class="input-group">
                    <label>المستوى الدراسي</label>
                    <input type="number" id="gradeLevel" min="1" max="12" placeholder="مثال: 2" value="2">
                </div>
                <div class="input-group">
                    <label>عدد الفصول</label>
                    <input type="number" id="gradeCount" min="1" max="20" placeholder="مثال: 8" value="8">
                </div>
            </div>
            
            <div class="actions">
                <button onclick="generateClassList()">
                    <i class="fas fa-cogs"></i>
                    <span>إنشاء قائمة الصفوف</span>
                </button>
            </div>
            
            <div class="grade-list" id="classListContainer">
                <h3>قائمة الصفوف المتاحة</h3>
                <div id="classList">
                    <!-- سيتم عرض الصفوف هنا -->
                </div>
            </div>
        </div>
    </div>
    
    <!-- تبويب تحليل النتائج -->
    <div id="analysis-tab" class="tab-content">
        <h2><i class="fas fa-chart-bar"></i> تحليل النتائج</h2>
        
        <div id="analysis-alert" class="alert warning hidden">
            <i class="fas fa-exclamation-circle"></i>
            <span>لا توجد بيانات لعرض التحليل</span>
        </div>
        
        <div class="summary-cards" id="summaryCards">
            <!-- سيتم إضافة بطاقات الملخص هنا -->
        </div>
        
        <div class="charts-container">
            <div class="chart-box">
                <h3>توزيع الطلاب حسب المستوى</h3>
                <canvas id="levelChart"></canvas>
            </div>
            <div class="chart-box">
                <h3>متوسط الدرجات حسب المادة</h3>
                <canvas id="subjectChart"></canvas>
            </div>
        </div>
        
        <div class="charts-container">
            <div class="chart-box">
                <h3>متوسط الدرجات حسب الفصل</h3>
                <canvas id="classChart"></canvas>
            </div>
            <div class="chart-box">
                <h3>توزيع الدرجات</h3>
                <canvas id="performanceChart"></canvas>
            </div>
        </div>
        
        <div class="section-title">تفاصيل النتائج حسب المستوى</div>
        <div class="table" id="levelDetailsTable">
            <!-- سيتم إضافة جدول تفاصيل المستويات هنا -->
        </div>
    </div>
    
    <!-- تبويب التقرير -->
    <div id="report-tab" class="tab-content">
        <h2><i class="fas fa-file-pdf"></i> تقرير PDF</h2>
        
        <div id="report-alert" class="alert hidden"></div>
        
        <div class="download-buttons">
            <button onclick="generatePDF()" class="whatsapp">
                <i class="fas fa-download"></i>
                <span>تحميل PDF</span>
            </button>
            <button onclick="shareViaWhatsApp()" class="whatsapp">
                <i class="fab fa-whatsapp"></i>
                <span>مشاركة عبر واتساب</span>
            </button>
            <button onclick="printReport()" class="secondary">
                <i class="fas fa-print"></i>
                <span>طباعة التقرير</span>
            </button>
        </div>
        
        <div class="page hidden" id="report">
            <div class="header">
                <div style="text-align: right;">
                    <strong>وزارة التعليم</strong><br>
                    <small>الإدارة العامة للتعليم</small>
                </div>
                <div style="text-align: center;">
                    <strong>مدرسة النخبة النموذجية</strong><br>
                    <small>التقرير التحليلي للنتائج</small>
                </div>
                <div style="text-align: left;">
                    <strong>الاختبار النهائي</strong><br>
                    <small>الفصل الدراسي الأول</small>
                </div>
            </div>
            
            <div class="title" id="reportTitle">تقرير تحليل نتائج الطلاب</div>
            
            <div class="info">
                <div class="box" id="reportGrade">المرحلة<span>متوسط</span></div>
                <div class="box" id="reportTerm">الفصل الدراسي<span>الأول 1447هـ</span></div>
                <div class="box" id="reportTotalStudents">عدد الطلاب<span>0</span></div>
            </div>
            
            <div class="summary" id="reportSummary">
                <!-- سيتم إضافة الملخص هنا -->
            </div>
            
            <div class="section-title">توزيع الطلاب حسب المستوى</div>
            <div class="table" id="reportLevelTable">
                <!-- سيتم إضافة جدول المستويات هنا -->
            </div>
            
            <div class="stats" id="reportStats">
                <!-- سيتم إضافة الإحصائيات هنا -->
            </div>
            
            <div class="section-title">تحليل النتائج حسب الفصول</div>
            <div class="charts-container" style="grid-template-columns: 1fr 1fr;">
                <div class="chart-box">
                    <h3>متوسط الدرجات حسب الفصل</h3>
                    <canvas id="reportClassChart"></canvas>
                </div>
                <div class="chart-box">
                    <h3>توزيع المتوسط حسب المادة</h3>
                    <canvas id="reportSubjectChart"></canvas>
                </div>
            </div>
            
            <div class="footer">
                <div style="text-align: center;">
                    <strong>مدير المدرسة</strong><br>
                    <small>.........................</small>
                </div>
                <div style="text-align: center;">
                    <strong>رئيس القسم</strong><br>
                    <small>.........................</small>
                </div>
                <div style="text-align: center;">
                    <strong>معلم المادة</strong><br>
                    <small>.........................</small>
                </div>
            </div>
        </div>
    </div>
</div>

<script>
// تخزين بيانات الطلاب والصفوف
let students = [];
let classes = [];
let currentSubject = 'الرياضيات';

// تهيئة قائمة الصفوف الافتراضية
function initializeClasses() {
    classes = [];
    const gradeLevel = 2; // ثاني متوسط
    const gradeCount = 8; // 8 فصول
    
    for (let i = 1; i <= gradeCount; i++) {
        // تحويل الرقم إلى الحرف العربي المقابل
        let arabicLetter;
        if (i === 1) arabicLetter = 'أ';
        else if (i === 2) arabicLetter = 'ب';
        else if (i === 3) arabicLetter = 'ج';
        else if (i === 4) arabicLetter = 'د';
        else if (i === 5) arabicLetter = 'هـ';
        else if (i === 6) arabicLetter = 'و';
        else if (i === 7) arabicLetter = 'ز';
        else if (i === 8) arabicLetter = 'ح';
        else if (i === 9) arabicLetter = 'ط';
        else arabicLetter = 'ي';
        
        classes.push(`${gradeLevel}/${arabicLetter}`);
    }
    
    updateClassSelect();
    renderClassList();
}

// تحديث قائمة الصفوف في select
function updateClassSelect() {
    const select = document.getElementById('className');
    select.innerHTML = '';
    
    classes.forEach(className => {
        const option = document.createElement('option');
        option.value = className;
        option.textContent = className;
        select.appendChild(option);
    });
}

// عرض قائمة الصفوف
function renderClassList() {
    const classList = document.getElementById('classList');
    if (classes.length === 0) {
        classList.innerHTML = '<p style="text-align: center; color: #666; padding: 20px;">لا توجد صفوف مضافة</p>';
        return;
    }
    
    let html = '<div style="display: grid; grid-template-columns: repeat(auto-fill, minmax(120px, 1fr)); gap: 10px;">';
    
    classes.forEach(className => {
        html += `
            <div class="grade-item" style="background: white; padding: 10px; border-radius: 6px; text-align: center;">
                <strong>${className}</strong>
            </div>
        `;
    });
    
    html += '</div>';
    classList.innerHTML = html;
}

// إنشاء قائمة الصفوف من المدخلات
function generateClassList() {
    const stage = document.getElementById('gradeStage').value.trim();
    const level = parseInt(document.getElementById('gradeLevel').value);
    const count = parseInt(document.getElementById('gradeCount').value);
    
    if (!stage || isNaN(level) || level < 1 || level > 12 || isNaN(count) || count < 1 || count > 20) {
        showAlert('grade-alert', 'يرجى إدخال بيانات صحيحة', 'error');
        return;
    }
    
    // تحديث عنوان المرحلة
    const gradeTitle = document.getElementById('reportGrade');
    if (gradeTitle) {
        gradeTitle.innerHTML = `المرحلة<span>${stage}</span>`;
    }
    
    // إنشاء قائمة الصفوف
    classes = [];
    const arabicLetters = ['أ', 'ب', 'ج', 'د', 'هـ', 'و', 'ز', 'ح', 'ط', 'ي', 'ك', 'ل', 'م', 'ن', 'س', 'ع', 'ف', 'ص', 'ق', 'ر'];
    
    for (let i = 0; i < count; i++) {
        if (i < arabicLetters.length) {
            classes.push(`${level}/${arabicLetters[i]}`);
        } else {
            classes.push(`${level}/${i + 1}`);
        }
    }
    
    updateClassSelect();
    renderClassList();
    showAlert('grade-alert', `تم إنشاء ${count} فصل بنجاح`, 'success');
}

// إعادة تعيين الصفوف
function resetGrades() {
    if (confirm('هل تريد إعادة تعيين قائمة الصفوف إلى الإعدادات الافتراضية؟')) {
        initializeClasses();
        showAlert('grade-alert', 'تم إعادة تعيين الصفوف بنجاح', 'success');
    }
}

// وظيفة عرض رسائل التنبيه
function showAlert(elementId, message, type = 'info') {
    const alertElement = document.getElementById(elementId);
    
    if (!alertElement) return;
    
    alertElement.innerHTML = '';
    
    const icon = type === 'success' ? 'fas fa-check-circle' : 
                 type === 'error' ? 'fas fa-times-circle' : 
                 type === 'warning' ? 'fas fa-exclamation-triangle' : 
                 'fas fa-info-circle';
    
    const alertClass = type === 'success' ? 'success' : 
                      type === 'error' ? 'error' : 
                      type === 'warning' ? 'warning' : '';
    
    alertElement.className = `alert ${alertClass}`;
    alertElement.innerHTML = `
        <i class="${icon}"></i>
        <span>${message}</span>
    `;
    
    alertElement.classList.remove('hidden');
    
    // إخفاء الرسالة بعد 5 ثواني
    setTimeout(() => {
        alertElement.classList.add('hidden');
    }, 5000);
}

// وظيفة تبديل التبويبات
function switchTab(tabName) {
    // إخفاء جميع المحتويات
    document.querySelectorAll('.tab-content').forEach(tab => {
        tab.classList.remove('active');
    });
    
    // إزالة النشط من جميع التبويبات
    document.querySelectorAll('.tab').forEach(tab => {
        tab.classList.remove('active');
    });
    
    // إظهار المحتوى المطلوب
    document.getElementById(tabName + '-tab').classList.add('active');
    
    // إضافة النشط للتبويب المطلوب
    document.querySelectorAll('.tab').forEach(tab => {
        if (tab.querySelector('span') && tab.querySelector('span').textContent.includes(getTabText(tabName))) {
            tab.classList.add('active');
        }
    });
    
    // إذا كان تبويب التحليل، قم بتحديث التحليل
    if (tabName === 'analysis') {
        updateAnalysis();
    }
    
    // إذا كان تبويب التقرير، قم بتحديث التقرير
    if (tabName === 'report') {
        updateReportPreview();
    }
    
    // إخفاء أي رسائل تنبيه
    document.querySelectorAll('.alert').forEach(alert => {
        alert.classList.add('hidden');
    });
}

function getTabText(tabName) {
    switch(tabName) {
        case 'input': return 'إدخال البيانات';
        case 'grades': return 'إدارة الصفوف';
        case 'analysis': return 'تحليل النتائج';
        case 'report': return 'تقرير PDF';
        default: return '';
    }
}

// وظيفة إضافة طالب جديد
function addStudent() {
    const name = document.getElementById('studentName').value.trim();
    const subject = document.getElementById('subject').value;
    const className = document.getElementById('className').value;
    const score = parseFloat(document.getElementById('score').value);
    
    if (!name || isNaN(score) || score < 0 || score > 40) {
        showAlert('alert-message', 'يرجى إدخال بيانات صحيحة (الدرجة من 0 إلى 40)', 'error');
        return;
    }
    
    if (classes.length === 0) {
        showAlert('alert-message', 'يرجى إضافة صفوف أولاً من تبويب إدارة الصفوف', 'warning');
        switchTab('grades');
        return;
    }
    
    // تحديد المستوى بناءً على الدرجة
    const level = getLevel(score);
    
    // إضافة الطالب إلى القائمة
    students.push({
        id: Date.now() + Math.random(),
        name,
        subject,
        className,
        score,
        level
    });
    
    // تحديث الجدول
    updateStudentsTable();
    
    // تفريغ الحقول
    document.getElementById('studentName').value = '';
    document.getElementById('score').value = '';
    
    // تركيز على اسم الطالب
    document.getElementById('studentName').focus();
    
    // إظهار رسالة تأكيد
    showAlert('alert-message', `تم إضافة الطالب ${name} بنجاح`, 'success');
    
    // تحديث تحليل النتائج إذا كان التبويب مفتوحاً
    if (document.getElementById('analysis-tab').classList.contains('active')) {
        updateAnalysis();
    }
}

// وظيفة الحصول على المستوى بناءً على الدرجة
function getLevel(score) {
    if (score >= 36) return {name: 'ممتاز', class: 'excellent'};
    if (score >= 32) return {name: 'جيد جدًا', class: 'verygood'};
    if (score >= 28) return {name: 'جيد', class: 'good'};
    if (score >= 20) return {name: 'مقبول', class: 'pass'};
    return {name: 'ضعيف', class: 'weak'};
}

// وظيفة تحديث جدول الطلاب
function updateStudentsTable() {
    const tbody = document.getElementById('studentsTableBody');
    
    if (students.length === 0) {
        tbody.innerHTML = '<tr><td colspan="7" style="text-align:center; padding:20px; color:#666;"><i class="fas fa-users-slash"></i> لا توجد بيانات، يرجى إضافة طلاب</td></tr>';
        return;
    }
    
    let html = '';
    
    students.forEach((student, index) => {
        html += `
            <tr>
                <td>${index + 1}</td>
                <td>${student.name}</td>
                <td>${student.subject}</td>
                <td>${student.className}</td>
                <td><strong>${student.score}</strong></td>
                <td><span class="level ${student.level.class}">${student.level.name}</span></td>
                <td><button class="delete-btn" onclick="deleteStudent('${student.id}')"><i class="fas fa-trash"></i> حذف</button></td>
            </tr>
        `;
    });
    
    tbody.innerHTML = html;
}

// وظيفة حذف طالب
function deleteStudent(id) {
    if (confirm('هل أنت متأكد من حذف هذا الطالب؟')) {
        students = students.filter(student => student.id !== id);
        updateStudentsTable();
        
        // تحديث التحليل إذا كان التبويب مفتوحاً
        if (document.getElementById('analysis-tab').classList.contains('active')) {
            updateAnalysis();
        }
        
        showAlert('alert-message', 'تم حذف الطالب بنجاح', 'success');
    }
}

// وظيفة تفريغ الحقول
function clearForm() {
    document.getElementById('studentName').value = '';
    document.getElementById('score').value = '';
    document.getElementById('studentName').focus();
}

// وظيفة إنشاء بيانات تجريبية
function generateSampleData() {
    const sampleNames = [
        'أحمد محمد', 'سارة علي', 'محمد حسن', 'فاطمة عبدالله', 'خالد إبراهيم',
        'نورة سعد', 'عبدالله راشد', 'لطيفة سالم', 'عمر ناصر', 'مريم خالد',
        'ياسر كمال', 'هدى محمود', 'بدر راشد', 'سلمى وليد', 'فهد صالح'
    ];
    
    const subjects = ['الرياضيات', 'اللغة العربية', 'اللغة الإنجليزية', 'العلوم', 'الدراسات الاجتماعية'];
    
    // التأكد من وجود صفوف
    if (classes.length === 0) {
        showAlert('alert-message', 'يرجى إنشاء صفوف أولاً من تبويب إدارة الصفوف', 'warning');
        switchTab('grades');
        return;
    }
    
    // إضافة 15 طالب عشوائيين
    for (let i = 0; i < 15; i++) {
        const name = sampleNames[Math.floor(Math.random() * sampleNames.length)];
        const subject = subjects[Math.floor(Math.random() * subjects.length)];
        const className = classes[Math.floor(Math.random() * classes.length)];
        const score = Math.floor(Math.random() * 41); // من 0 إلى 40
        
        // تحديد المستوى بناءً على الدرجة
        const level = getLevel(score);
        
        students.push({
            id: Date.now() + Math.random() + i,
            name: `${name} ${i + 1}`,
            subject,
            className,
            score,
            level
        });
    }
    
    updateStudentsTable();
    showAlert('alert-message', 'تم إضافة 15 طالب عشوائيين بنجاح', 'success');
    
    // تحديث التحليل إذا كان التبويب مفتوحاً
    if (document.getElementById('analysis-tab').classList.contains('active')) {
        updateAnalysis();
    }
}

// وظيفة تحديث التحليل
function updateAnalysis() {
    const analysisAlert = document.getElementById('analysis-alert');
    
    if (students.length === 0) {
        analysisAlert.classList.remove('hidden');
        document.getElementById('summaryCards').innerHTML = '';
        document.getElementById('levelDetailsTable').innerHTML = '';
        return;
    }
    
    analysisAlert.classList.add('hidden');
    
    // حساب الإحصائيات
    const totalStudents = students.length;
    const avgScore = students.reduce((sum, student) => sum + student.score, 0) / totalStudents;
    const passedStudents = students.filter(student => student.score >= 20).length;
    const passRate = (passedStudents / totalStudents * 100).toFixed(1);
    
    // حساب توزيع المستويات
    const levelCounts = {
        'ممتاز': 0, 'جيد جدًا': 0, 'جيد': 0, 'مقبول': 0, 'ضعيف': 0
    };
    
    students.forEach(student => {
        levelCounts[student.level.name]++;
    });
    
    // حساب توزيع المواد
    const subjectCounts = {};
    students.forEach(student => {
        if (!subjectCounts[student.subject]) {
            subjectCounts[student.subject] = {count: 0, totalScore: 0};
        }
        subjectCounts[student.subject].count++;
        subjectCounts[student.subject].totalScore += student.score;
    });
    
    // حساب توزيع الفصول
    const classCounts = {};
    students.forEach(student => {
        if (!classCounts[student.className]) {
            classCounts[student.className] = {count: 0, totalScore: 0};
        }
        classCounts[student.className].count++;
        classCounts[student.className].totalScore += student.score;
    });
    
    // تحديث بطاقات الملخص
    updateSummaryCards(totalStudents, avgScore, passRate, levelCounts);
    
    // تحديث الرسوم البيانية
    updateCharts(levelCounts, subjectCounts, classCounts);
    
    // تحديث جدول تفاصيل المستويات
    updateLevelDetailsTable(levelCounts);
}

// تحديث بطاقات الملخص
function updateSummaryCards(totalStudents, avgScore, passRate, levelCounts) {
    const highestLevel = Object.entries(levelCounts).reduce((a, b) => a[1] > b[1] ? a : b)[0];
    const highestCount = levelCounts[highestLevel];
    
    document.getElementById('summaryCards').innerHTML = `
        <div class="summary-card">
            <h3><i class="fas fa-users"></i> عدد الطلاب</h3>
            <div class="value">${totalStudents}</div>
            <div class="subtext">طالب</div>
        </div>
        <div class="summary-card">
            <h3><i class="fas fa-chart-line"></i> متوسط الدرجات</h3>
            <div class="value">${avgScore.toFixed(1)}</div>
            <div class="subtext">من 40</div>
        </div>
        <div class="summary-card">
            <h3><i class="fas fa-percentage"></i> نسبة النجاح</h3>
            <div class="value">${passRate}%</div>
            <div class="subtext">${passedStudents} طالب</div>
        </div>
        <div class="summary-card">
            <h3><i class="fas fa-trophy"></i> أعلى مستوى</h3>
            <div class="value">${highestLevel}</div>
            <div class="subtext">${highestCount} طالب</div>
        </div>
    `;
}

// تحديث الرسوم البيانية
function updateCharts(levelCounts, subjectCounts, classCounts) {
    // تدمير الرسوم البيانية القديمة إن وجدت
    const charts = ['levelChart', 'subjectChart', 'classChart', 'performanceChart'];
    charts.forEach(chartId => {
        const chart = Chart.getChart(chartId);
        if (chart) chart.destroy();
    });
    
    // رسم بياني للمستويات
    new Chart(document.getElementById('levelChart'), {
        type: 'doughnut',
        data: {
            labels: Object.keys(levelCounts),
            datasets: [{
                data: Object.values(levelCounts),
                backgroundColor: ['#4caf50', '#009688', '#2196f3', '#ff9800', '#f44336'],
                borderWidth: 1,
                borderColor: '#fff'
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: { 
                    position: 'bottom', 
                    labels: { 
                        font: { size: 11 },
                        padding: 15
                    }
                },
                tooltip: {
                    callbacks: {
                        label: (ctx) => `${ctx.label}: ${ctx.raw} طالب (${((ctx.raw/students.length)*100).toFixed(1)}%)`
                    }
                }
            }
        }
    });
    
    // رسم بياني للمواد
    const subjectLabels = Object.keys(subjectCounts);
    const subjectAverages = subjectLabels.map(subject => 
        (subjectCounts[subject].totalScore / subjectCounts[subject].count).toFixed(1)
    );
    
    new Chart(document.getElementById('subjectChart'), {
        type: 'bar',
        data: {
            labels: subjectLabels,
            datasets: [{
                label: 'متوسط الدرجة',
                data: subjectAverages,
                backgroundColor: '#1a5c9e',
                borderRadius: 6,
                borderWidth: 1
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: { display: false }
            },
            scales: {
                y: {
                    beginAtZero: true,
                    max: 40,
                    ticks: { 
                        stepSize: 5, 
                        font: { size: 11 },
                        callback: value => value + ' درجة'
                    },
                    grid: { color: 'rgba(0,0,0,0.05)' }
                },
                x: {
                    ticks: { font: { size: 11 } },
                    grid: { display: false }
                }
            }
        }
    });
    
    // رسم بياني للفصول
    const classLabels = Object.keys(classCounts);
    const classAverages = classLabels.map(className => 
        (classCounts[className].totalScore / classCounts[className].count).toFixed(1)
    );
    
    new Chart(document.getElementById('classChart'), {
        type: 'bar',
        data: {
            labels: classLabels,
            datasets: [{
                label: 'متوسط الدرجة',
                data: classAverages,
                backgroundColor: '#009688',
                borderRadius: 6,
                borderWidth: 1
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: { display: false }
            },
            scales: {
                y: {
                    beginAtZero: true,
                    max: 40,
                    ticks: { 
                        stepSize: 5, 
                        font: { size: 11 },
                        callback: value => value + ' درجة'
                    },
                    grid: { color: 'rgba(0,0,0,0.05)' }
                },
                x: {
                    ticks: { font: { size: 11 } },
                    grid: { display: false }
                }
            }
        }
    });
    
    // رسم بياني للأداء
    const performanceLabels = ['0-19', '20-27', '28-31', '32-35', '36-40'];
    const performanceData = [
        levelCounts['ضعيف'] || 0,
        levelCounts['مقبول'] || 0,
        levelCounts['جيد'] || 0,
        levelCounts['جيد جدًا'] || 0,
        levelCounts['ممتاز'] || 0
    ];
    
    new Chart(document.getElementById('performanceChart'), {
        type: 'line',
        data: {
            labels: performanceLabels,
            datasets: [{
                label: 'عدد الطلاب',
                data: performanceData,
                borderColor: '#f44336',
                backgroundColor: 'rgba(244, 67, 54, 0.1)',
                fill: true,
                tension: 0.3,
                borderWidth: 2,
                pointBackgroundColor: '#f44336',
                pointRadius: 4
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: { display: false }
            },
            scales: {
                y: {
                    beginAtZero: true,
                    ticks: { 
                        stepSize: 1, 
                        font: { size: 11 }
                    },
                    grid: { color: 'rgba(0,0,0,0.05)' }
                },
                x: {
                    ticks: { font: { size: 11 } },
                    grid: { color: 'rgba(0,0,0,0.05)' }
                }
            }
        }
    });
}

// تحديث جدول تفاصيل المستويات
function updateLevelDetailsTable(levelCounts) {
    const levelRanges = {
        'ممتاز': '36 - 40',
        'جيد جدًا': '32 - 35.99',
        'جيد': '28 - 31.99',
        'مقبول': '20 - 27.99',
        'ضعيف': '0 - 19.99'
    };
    
    const levelClasses = {
        'ممتاز': 'excellent',
        'جيد جدًا': 'verygood',
        'جيد': 'good',
        'مقبول': 'pass',
        'ضعيف': 'weak'
    };
    
    let tableHTML = `
        <div class="row">
            <div class="cell">عدد</div>
            <div class="cell">النطاق</div>
            <div class="cell">المستوى</div>
        </div>
    `;
    
    const levels = ['ممتاز', 'جيد جدًا', 'جيد', 'مقبول', 'ضعيف'];
    
    levels.forEach(level => {
        const count = levelCounts[level] || 0;
        const percentage = ((count / students.length) * 100).toFixed(1);
        tableHTML += `
            <div class="row">
                <div class="cell"><strong>${count}</strong> <small>(${percentage}%)</small></div>
                <div class="cell">${levelRanges[level]}</div>
                <div class="cell"><span class="level ${levelClasses[level]}">${level}</span></div>
            </div>
        `;
    });
    
    document.getElementById('levelDetailsTable').innerHTML = tableHTML;
}

// تحديث معاينة التقرير
function updateReportPreview() {
    const reportAlert = document.getElementById('report-alert');
    const reportElement = document.getElementById('report');
    
    if (students.length === 0) {
        reportAlert.classList.remove('hidden');
        reportAlert.className = 'alert warning';
        reportAlert.innerHTML = '<i class="fas fa-exclamation-circle"></i><span>لا توجد بيانات لإنشاء التقرير</span>';
        reportElement.classList.add('hidden');
        return;
    }
    
    reportAlert.classList.add('hidden');
    reportElement.classList.remove('hidden');
    
    // تحديث عنوان التقرير
    const subjects = [...new Set(students.map(s => s.subject))];
    const subjectText = subjects.length > 1 ? 'المواد الدراسية' : subjects[0];
    document.getElementById('reportTitle').textContent = `تقرير تحليل نتائج ${subjectText}`;
    
    // تحديث معلومات التقرير
    document.getElementById('reportTotalStudents').innerHTML = `عدد الطلاب<span>${students.length}</span>`;
    
    // تحديث ملخص التقرير
    const avgScore = students.reduce((sum, student) => sum + student.score, 0) / students.length;
    const passedStudents = students.filter(student => student.score >= 20).length;
    const passRate = (passedStudents / students.length * 100).toFixed(1);
    
    let overallLevel = 'جيد';
    if (avgScore >= 32) overallLevel = 'جيد جدًا';
    if (avgScore >= 36) overallLevel = 'ممتاز';
    if (avgScore < 28) overallLevel = 'مقبول';
    if (avgScore < 20) overallLevel = 'ضعيف';
    
    const weakPercentage = ((students.filter(s => s.score < 20).length / students.length) * 100).toFixed(1);
    
    document.getElementById('reportSummary').innerHTML = `
        <strong>مستوى الأداء العام:</strong> ${overallLevel}<br>
        <strong>نسبة الطلاب دون مستوى الإتقان:</strong> ${weakPercentage}%<br>
        <strong>نسبة النجاح:</strong> ${passRate}%<br>
        <strong>قراءة تحليلية مختصرة:</strong>
        تعكس النتائج ${overallLevel === 'ممتاز' ? 'تميزاً' : overallLevel === 'جيد جدًا' ? 'أداءً قوياً' : 'استقراراً'} في مستوى التحصيل مع وجود ${weakPercentage > 30 ? 'فئة' : 'فئة محدودة'} تحتاج دعمًا إضافيًا.
    `;
    
    // تحديث جدول المستويات في التقرير
    updateReportLevelTable();
    
    // تحديث إحصائيات التقرير
    document.getElementById('reportStats').innerHTML = `
        <div class="stat">عدد الطلاب<strong>${students.length}</strong></div>
        <div class="stat">المتوسط<strong>${avgScore.toFixed(1)}</strong></div>
        <div class="stat">نسبة النجاح<strong>${passRate}%</strong></div>
    `;
    
    // تحديث الرسوم البيانية في التقرير
    updateReportCharts();
}

// تحديث جدول المستويات في التقرير
function updateReportLevelTable() {
    const levelCounts = {
        'ممتاز': 0, 'جيد جدًا': 0, 'جيد': 0, 'مقبول': 0, 'ضعيف': 0
    };
    
    students.forEach(student => {
        levelCounts[student.level.name]++;
    });
    
    const levelRanges = {
        'ممتاز': '36 - 40',
        'جيد جدًا': '32 - 35.99',
        'جيد': '28 - 31.99',
        'مقبول': '20 - 27.99',
        'ضعيف': '0 - 19.99'
    };
    
    const levelClasses = {
        'ممتاز': 'excellent',
        'جيد جدًا': 'verygood',
        'جيد': 'good',
        'مقبول': 'pass',
        'ضعيف': 'weak'
    };
    
    let tableHTML = `
        <div class="row">
            <div class="cell">عدد</div>
            <div class="cell">النطاق</div>
            <div class="cell">المستوى</div>
        </div>
    `;
    
    const levels = ['ممتاز', 'جيد جدًا', 'جيد', 'مقبول', 'ضعيف'];
    
    levels.forEach(level => {
        const count = levelCounts[level] || 0;
        tableHTML += `
            <div class="row">
                <div class="cell">${count}</div>
                <div class="cell">${levelRanges[level]}</div>
                <div class="cell"><span class="level ${levelClasses[level]}">${level}</span></div>
            </div>
        `;
    });
    
    document.getElementById('reportLevelTable').innerHTML = tableHTML;
}

// تحديث الرسوم البيانية في التقرير
function updateReportCharts() {
    // تدمير الرسوم البيانية القديمة إن وجدت
    const reportCharts = ['reportClassChart', 'reportSubjectChart'];
    reportCharts.forEach(chartId => {
        const chart = Chart.getChart(chartId);
        if (chart) chart.destroy();
    });
    
    // حساب توزيع الفصول
    const classCounts = {};
    students.forEach(student => {
        if (!classCounts[student.className]) {
            classCounts[student.className] = {count: 0, totalScore: 0};
        }
        classCounts[student.className].count++;
        classCounts[student.className].totalScore += student.score;
    });
    
    // حساب توزيع المواد
    const subjectCounts = {};
    students.forEach(student => {
        if (!subjectCounts[student.subject]) {
            subjectCounts[student.subject] = {count: 0, totalScore: 0};
        }
        subjectCounts[student.subject].count++;
        subjectCounts[student.subject].totalScore += student.score;
    });
    
    // رسم بياني للفصول
    const classLabels = Object.keys(classCounts).sort();
    const classAverages = classLabels.map(className => 
        (classCounts[className].totalScore / classCounts[className].count).toFixed(1)
    );
    
    new Chart(document.getElementById('reportClassChart'), {
        type: 'bar',
        data: {
            labels: classLabels,
            datasets: [{
                label: 'متوسط الدرجة',
                data: classAverages,
                backgroundColor: '#1a5c9e',
                borderRadius: 4,
                borderWidth: 1
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: { display: false },
                title: { display: false }
            },
            scales: {
                y: {
                    beginAtZero: true,
                    max: 40,
                    ticks: { 
                        stepSize: 5, 
                        font: { size: 10 },
                        callback: value => value + ' درجة'
                    },
                    grid: { color: 'rgba(0,0,0,0.05)' }
                },
                x: {
                    ticks: { font: { size: 10 } },
                    grid: { display: false }
                }
            }
        }
    });
    
    // رسم بياني للمواد
    const subjectLabels = Object.keys(subjectCounts);
    const subjectAverages = subjectLabels.map(subject => 
        (subjectCounts[subject].totalScore / subjectCounts[subject].count).toFixed(1)
    );
    
    new Chart(document.getElementById('reportSubjectChart'), {
        type: 'pie',
        data: {
            labels: subjectLabels,
            datasets: [{
                data: subjectAverages,
                backgroundColor: ['#4caf50', '#009688', '#2196f3', '#ff9800', '#f44336', '#9c27b0', '#795548', '#607d8b'],
                borderWidth: 1,
                borderColor: '#fff'
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: { 
                    position: 'bottom', 
                    labels: { 
                        font: { size: 9 },
                        padding: 10
                    }
                },
                tooltip: {
                    callbacks: {
                        label: (ctx) => `${ctx.label}: ${ctx.raw} درجة`
                    }
                }
            }
        }
    });
}

// وظيفة إنشاء PDF
async function generatePDF() {
    if (students.length === 0) {
        showAlert('report-alert', 'لا توجد بيانات لإنشاء تقرير', 'error');
        return;
    }
    
    try {
        // عرض مؤشر التحميل
        const originalButton = event.target.closest('button');
        const originalHTML = originalButton.innerHTML;
        originalButton.innerHTML = '<i class="fas fa-spinner fa-spin"></i><span>جاري التحميل...</span>';
        originalButton.disabled = true;
        
        const element = document.getElementById("report");
        const canvas = await html2canvas(element, {
            scale: 2,
            backgroundColor: "#fff",
            useCORS: true,
            logging: false
        });
        
        const imgData = canvas.toDataURL("image/jpeg", 1.0);
        const { jsPDF } = window.jspdf;
        const pdf = new jsPDF("p", "mm", "a4");
        
        pdf.addImage(imgData, "JPEG", 0, 0, 210, 297);
        
        // حفظ الملف مع اسم مناسب
        const date = new Date().toISOString().slice(0, 10);
        const filename = `تقرير_النتائج_${date}.pdf`;
        
        pdf.save(filename);
        
        showAlert('report-alert', 'تم إنشاء التقرير بنجاح وحفظه في جهازك', 'success');
        
    } catch (error) {
        console.error('خطأ في إنشاء PDF:', error);
        showAlert('report-alert', 'حدث خطأ أثناء إنشاء التقرير', 'error');
    } finally {
        // إعادة حالة الزر
        if (originalButton) {
            originalButton.innerHTML = originalHTML;
            originalButton.disabled = false;
        }
    }
}

// وظيفة المشاركة عبر واتساب
async function shareViaWhatsApp() {
    if (students.length === 0) {
        showAlert('report-alert', 'لا توجد بيانات للمشاركة', 'error');
        return;
    }
    
    try {
        // عرض مؤشر التحميل
        const originalButton = event.target.closest('button');
        const originalHTML = originalButton.innerHTML;
        originalButton.innerHTML = '<i class="fas fa-spinner fa-spin"></i><span>جاري التحضير...</span>';
        originalButton.disabled = true;
        
        const element = document.getElementById("report");
        const canvas = await html2canvas(element, {
            scale: 2,
            backgroundColor: "#fff",
            useCORS: true,
            logging: false
        });
        
        const imgData = canvas.toDataURL("image/jpeg", 0.8);
        const { jsPDF } = window.jspdf;
        const pdf = new jsPDF("p", "mm", "a4");
        
        pdf.addImage(imgData, "JPEG", 0, 0, 210, 297);
        const blob = pdf.output("blob");
        
        // إنشاء ملف PDF
        const date = new Date().toISOString().slice(0, 10);
        const filename = `تقرير_النتائج_${date}.pdf`;
        const file = new File([blob], filename, { type: "application/pdf" });
        
        // التحقق من إمكانية المشاركة
        if (navigator.share && navigator.canShare && navigator.canShare({ files: [file] })) {
            try {
                await navigator.share({
                    files: [file],
                    title: "تقرير نتائج الطلاب",
                    text: "تقرير تحليل نتائج الطلاب - تم إنشاؤه عبر أداة تحليل النتائج"
                });
                
                showAlert('report-alert', 'تم مشاركة التقرير بنجاح', 'success');
                
            } catch (shareError) {
                console.error('خطأ في المشاركة:', shareError);
                
                // إذا فشلت المشاركة، نعرض خيارات بديلة
                if (shareError.name !== 'AbortError') {
                    // حفظ الملف بدلاً من المشاركة
                    pdf.save(filename);
                    showAlert('report-alert', 'تم حفظ التقرير في جهازك', 'success');
                }
            }
        } else {
            // إذا كان المتصفح لا يدعم المشاركة، نعرض رابط واتساب
            const text = encodeURIComponent("تقرير تحليل نتائج الطلاب\n\nتم إنشاء التقرير عبر أداة تحليل النتائج");
            const whatsappUrl = `https://wa.me/?text=${text}`;
            
            // حفظ الملف أولاً
            pdf.save(filename);
            
            // فتح واتساب في نافذة جديدة
            window.open(whatsappUrl, '_blank');
            
            showAlert('report-alert', 'تم حفظ التقرير، يمكنك الآن مشاركته عبر واتساب', 'success');
        }
        
    } catch (error) {
        console.error('خطأ في عملية المشاركة:', error);
        showAlert('report-alert', 'حدث خطأ أثناء محاولة المشاركة', 'error');
    } finally {
        // إعادة حالة الزر
        if (originalButton) {
            originalButton.innerHTML = originalHTML;
            originalButton.disabled = false;
        }
    }
}

// وظيفة الطباعة
function printReport() {
    if (students.length === 0) {
        showAlert('report-alert', 'لا توجد بيانات للطباعة', 'error');
        return;
    }
    
    // إظهار التقرير قبل الطباعة
    document.getElementById('report').classList.remove('hidden');
    
    // الانتظار قليلاً ثم الطباعة
    setTimeout(() => {
        window.print();
    }, 500);
}

// تهيئة التطبيق عند تحميل الصفحة
document.addEventListener('DOMContentLoaded', function() {
    // تهيئة قائمة الصفوف
    initializeClasses();
    
    // تحديث جدول الطلاب
    updateStudentsTable();
    
    // تهيئة التحليل
    updateAnalysis();
    
    // إضافة مستمعين للأحداث
    document.getElementById('studentName').addEventListener('keypress', function(e) {
        if (e.key === 'Enter') {
            addStudent();
        }
    });
    
    document.getElementById('score').addEventListener('keypress', function(e) {
        if (e.key === 'Enter') {
            addStudent();
        }
    });
    
    // تحسين العرض على الأجهزة المحمولة
    if ('ontouchstart' in window) {
        document.body.classList.add('touch-device');
    }
    
    // منع التكبير المزدوج على الهواتف
    let lastTouchEnd = 0;
    document.addEventListener('touchend', function(event) {
        const now = (new Date()).getTime();
        if (now - lastTouchEnd <= 300) {
            event.preventDefault();
        }
        lastTouchEnd = now;
    }, false);
    
    // تحسين الأداء على iOS
    if (navigator.userAgent.match(/iPhone|iPad|iPod/i)) {
        document.body.style.webkitTransform = 'translate3d(0,0,0)';
    }
    
    // دعم وضع ملء الشاشة على الهواتف
    if ('standalone' in navigator && !navigator.standalone) {
        const installPrompt = document.createElement('div');
        installPrompt.className = 'alert success';
        installPrompt.innerHTML = '<i class="fas fa-mobile-alt"></i><span>لتجربة أفضل، يمكنك إضافة هذه الصفحة إلى الشاشة الرئيسية</span>';
        document.querySelector('.container').prepend(installPrompt);
        
        setTimeout(() => {
            installPrompt.remove();
        }, 5000);
    }
});

// دعم وضع عدم الاتصال
if ('serviceWorker' in navigator) {
    window.addEventListener('load', function() {
        navigator.serviceWorker.register('/sw.js').catch(function(error) {
            console.log('Service Worker registration failed:', error);
        });
    });
}

// حفظ البيانات في التخزين المحلي
function saveToLocalStorage() {
    try {
        const data = {
            students: students,
            classes: classes,
            lastUpdated: new Date().toISOString()
        };
        localStorage.setItem('studentResultsData', JSON.stringify(data));
    } catch (error) {
        console.error('خطأ في حفظ البيانات:', error);
    }
}

// تحميل البيانات من التخزين المحلي
function loadFromLocalStorage() {
    try {
        const savedData = localStorage.getItem('studentResultsData');
        if (savedData) {
            const data = JSON.parse(savedData);
            students = data.students || [];
            classes = data.classes || [];
            
            if (classes.length === 0) {
                initializeClasses();
            } else {
                updateClassSelect();
                renderClassList();
            }
            
            updateStudentsTable();
            updateAnalysis();
        }
    } catch (error) {
        console.error('خطأ في تحميل البيانات:', error);
    }
}

// حفظ البيانات تلقائياً عند التغيير
let saveTimeout;
function autoSave() {
    clearTimeout(saveTimeout);
    saveTimeout = setTimeout(saveToLocalStorage, 1000);
}

// تعديل الدوال لإضافة autoSave
const originalAddStudent = addStudent;
addStudent = function() {
    originalAddStudent();
    autoSave();
};

const originalDeleteStudent = deleteStudent;
deleteStudent = function(id) {
    originalDeleteStudent(id);
    autoSave();
};

const originalGenerateSampleData = generateSampleData;
generateSampleData = function() {
    originalGenerateSampleData();
    autoSave();
};

// تحميل البيانات عند بدء التشغيل
window.addEventListener('load', loadFromLocalStorage);
</script>

</body>
</html>
