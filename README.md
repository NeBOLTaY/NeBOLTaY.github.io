<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>График смен работников</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/air-datepicker/2.2.3/css/datepicker.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/air-datepicker/2.2.3/js/datepicker.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/air-datepicker/2.2.3/js/i18n/datepicker.ru.js"></script>
    <script src="https://cdn.sheetjs.com/xlsx-0.20.0/package/dist/xlsx.full.min.js"></script>
    
    <style>
        :root {
            --logo-spin-duration: 10s;
        }

        @keyframes spin {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }

        @keyframes pulsate {
            0% { transform: scale(1); opacity: 0.9; }
            50% { transform: scale(1.1); opacity: 1; }
            100% { transform: scale(1); opacity: 0.9; }
        }

        @keyframes pulseHeart {
            0% { transform: scale(1); }
            15% { transform: scale(1.1); }
            30% { transform: scale(0.9); }
            45% { transform: scale(1.05); }
            60% { transform: scale(0.95); }
            75% { transform: scale(1.02); }
            100% { transform: scale(1); }
        }

        button:not(.close-modal):not(.btn-clear) {
            padding: 8px 18px !important;
            cursor: pointer;
            border: none !important;
            border-radius: 5px;
            background-color: #e0e0e0 !important;
            color: #333 !important;
            transition: all 0.2s ease;
            font-size: 13px;
            margin: 2px;
        }

body.dark-theme button:not(.close-modal):not(.btn-clear) {
    background-color: #2b5278 !important;
    color: #ffffff !important;
}

        button:not(.close-modal):not(.btn-clear):hover {
            background-color: #c8e6c9 !important;
            color: #2e7d32 !important;
        }

body.dark-theme button:not(.close-modal):not(.btn-clear):hover {
    background-color: #3d6c99 !important;
    color: #ffffff !important;
}

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 10px;
            background-color: #f4f4f4;
            color: #333;
            transition: background-color 0.3s, color 0.3s;
            font-size: 14px;
            display: flex;
            flex-direction: column;
            min-height: 100vh;
        }

body.dark-theme {
    background-color: #0e1621;
    color: #e1e1e1;
}

        .container {
            margin: auto;
            background-color: #fff;
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            transition: background-color 0.3s;
            width: 100%;
            max-width: 1400px;
            box-sizing: border-box;
            flex-grow: 1;
        }

body.dark-theme .container {
    background-color: #17212b;
    box-shadow: 0 2px 5px rgba(0,0,0,0.4);
}

        .header {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            border-bottom: 1px solid #ccc;
            padding-bottom: 15px;
            margin-bottom: 15px;
        }

        body.dark-theme .header {
            border-bottom-color: #555;
        }

        .header-left {
            display: flex;
            align-items: center;
            gap: 20px;
        }

        .header-title {
            font-size: 2em;
            color: inherit;
            transition: color 0.3s;
        }

        body.dark-theme .header-title {
            color: #f4f4f4;
        }

        .logo {
            height: 120px;
            opacity: 0.9;
            flex-shrink: 0;
            border-radius: 50%;
            cursor: pointer;
            animation: spin var(--logo-spin-duration) linear infinite,
                       pulsate 1.6s ease-in-out infinite;
        }

        .logo:hover {
            animation: pulseHeart 1.6s ease-in-out infinite;
            animation-delay: 0.35s;
        }

        .controls {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
        }

        .month-navigation {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            align-items: center;
            margin-bottom: 15px;
            gap: 10px;
        }

        #currentMonth {
            text-align: center;
            font-size: 1.1em;
            font-weight: bold;
        }

        #calendar {
            width: 100%;
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 10px;
            table-layout: fixed;
            min-width: 600px;
        }

        th, td {
            border: 1px solid #ddd;
            padding: 5px 3px;
            text-align: center;
            font-size: 10px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        body.dark-theme th, body.dark-theme td {
            border-color: #555;
        }

        th {
            background-color: #f2f2f2;
            font-weight: bold;
            position: sticky;
            top: 0;
            z-index: 1;
        }

body.dark-theme th {
    background-color: #2b5278;
    color: #ffffff;
}

        td:first-child, th:first-child {
            text-align: left;
            white-space: nowrap;
            min-width: 220px;
            width: 230px;
            max-width: 300px;
            position: sticky;
            left: 0;
            background-color: #fff;
            z-index: 2;
            vertical-align: middle;
            padding: 5px 10px;
            border-right: 2px solid #bbb;
        }

body.dark-theme td:first-child {
    background-color: #17212b;
    border-right-color: #2b5278;
}

        th:first-child {
            z-index: 3;
            background-color: #f2f2f2;
        }

body.dark-theme th:first-child {
    background-color: #2b5278;
    border-right-color: #2b5278;
}

        .employee-info {
            display: flex;
            align-items: center;
            gap: 8px;
            width: 100%;
            position: relative;
            min-width: 0;
        }

        .employee-number {
            display: none;
            position: absolute;
            bottom: calc(100% + 5px);
            left: 0;
            transform: none;
            background: rgba(255, 255, 255, 0.95);
            padding: 4px 8px;
            border-radius: 4px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
            z-index: 10;
            font-size: 0.8em;
            white-space: nowrap;
            pointer-events: none;
            border: 1px solid #ddd;
        }

        .employee-name {
            flex: 1;
            min-width: 0;
            overflow: hidden;
            text-overflow: ellipsis;
            position: relative;
            padding-right: 10px;
            white-space: nowrap;
        }

        .employee-name:hover .employee-number {
            display: block;
        }

        body.dark-theme .employee-number {
            background: rgba(50, 50, 50, 0.95);
            color: #fff;
            border-color: #666;
        }

        .action-buttons { 
            display: flex;
            gap: 4px;
            flex-shrink: 0;
            margin-left: auto;
            position: relative;
            z-index: 1;
        }

        .action-buttons button {
            padding: 8px 18px !important;
            font-size: 0.95em !important;
            border: none !important;
            background-color: #4CAF50 !important;
            color: white !important;
            border-radius: 5px;
            line-height: 1;
            cursor: pointer;
            transition: background-color 0.2s;
        }

body.dark-theme .action-buttons button {
    background-color: #2b5278 !important;
    color: #ffffff !important;
}

        .action-buttons button:hover {
            background-color: #45a049 !important;
        }

body.dark-theme .action-buttons button:hover {
    background-color: #3d6c99 !important;
}

        body.dark-theme .action-buttons button {
            border-color: #555 !important;
            background-color: #4a4a4a !important;
            color: #bbb;
        }

        .action-buttons button:hover { color: #000; }
        body.dark-theme .action-buttons button:hover { color: #fff; }

        th:nth-last-child(1), td:nth-last-child(1),
        th:nth-last-child(2), td:nth-last-child(2) {
            font-weight: bold;
            min-width: 50px;
            width: 60px;
            white-space: normal;
            font-size: 10px;
        }

        .weekend { background-color: #b0b0b0; }
        .vacation { background-color: #d4edda; color: #155724; }
        .sick { background-color: #f8d7da; color: #721c24; }
        .shortened { 
            background-color: #fff3cd; 
            color: #856404; 
        }
body.dark-theme .weekend {
    background-color: #1a2633;
    color: #7d8fa1;
}
body.dark-theme .vacation {
    background-color: #1d3b2a;
    color: #4db37d;
}
body.dark-theme .sick {
    background-color: #3b1e1e;
    color: #d46c6c;
}
body.dark-theme .shortened {
    background-color: #3a2e1b;
    color: #d4b56c;
}

        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0, 0, 0, 0.6);
            align-items: center;
            justify-content: center;
            padding: 10px;
        }

        .modal-content {
            background-color: #fefefe;
            margin: auto;
            padding: 15px 20px;
            border: 1px solid #888;
            width: 95%;
            max-width: 500px;
            border-radius: 8px;
            position: relative;
        }

body.dark-theme .modal-content {
    background-color: #17212b;
    border-color: #2b5278;
    color: #e1e1e1;
}
body.dark-theme input,
body.dark-theme select {
    background-color: #22303d;
    border-color: #2b5278;
    color: #e1e1e1;
}

        .close-modal {
            color: #aaa;
            position: absolute;
            top: 8px;
            right: 12px;
            font-size: 26px;
            font-weight: bold;
            cursor: pointer;
            line-height: 1;
        }

        .close-modal:hover, .close-modal:focus { color: black; }
body.dark-theme .close-modal {
    color: #7d8fa1;
}
body.dark-theme .close-modal:hover,
body.dark-theme .close-modal:focus {
    color: #ffffff;
}

        .form-group { margin-bottom: 12px; }
        .form-group label {
            display: block;
            margin-bottom: 4px;
            font-weight: bold;
            font-size: 0.95em;
        }

        .form-group input[type="text"],
        .form-group input[type="date"],
        .form-group input[type="number"],
        .form-group select {
            width: 100%;
            padding: 8px 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
            font-size: 1em;
        }

        .form-group-inline {
            display: flex;
            gap: 10px;
            align-items: flex-end;
        }

        .form-group-inline > div { flex: 1; }
        .form-group-inline label { display: none; }
        .form-group-inline input[type="number"] {
            width: 80px;
            flex: 0 0 80px;
        }

        body.dark-theme .form-group input,
        body.dark-theme .form-group select {
            background-color: #555;
            border-color: #666;
            color: #f1f1f1;
        }

        .modal .action-buttons {
            justify-content: flex-end;
            margin-top: 15px;
        }

        .modal button:not(.close-modal) {
            padding: 8px 18px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.2s;
            font-size: 0.95em;
        }

        body.dark-theme .modal button:not(.close-modal) {
            background-color: #66bb6a;
        }

        .modal button:not(.close-modal):hover {
            background-color: #45a049;
        }

        body.dark-theme .modal button:not(.close-modal):hover {
            background-color: #5eac61;
        }

        #lunchModalContent { max-width: 600px; }
        #lunchTable {
            width: 100%;
            margin-bottom: 15px;
        }

        #lunchTable th,
        #lunchTable td {
            text-align: left;
            padding: 6px;
            font-size: 0.9em;
        }

        #lunchTable input[type="number"] {
            width: 50px;
            padding: 4px;
            font-size: 1em;
        }

        footer {
            text-align: center;
            padding: 15px 10px;
            margin-top: 20px;
            font-size: 0.85em;
            color: #777;
            border-top: 1px solid #ddd;
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 8px;
        }

        body.dark-theme footer {
            color: #aaa;
            border-top-color: #555;
        }

        .footer-logo {
            height: 30px;
            width: 30px;
            display: inline-block;
            vertical-align: middle;
            border-radius: 50%;
        }

        #datepicker {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
        }

        .datepicker {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            box-shadow: 0 4px 12px rgba(0,0,0,0.2);
        }

body.dark-theme .datepicker {
    background: #17212b;
    border-color: #2b5278;
    color: #e1e1e1 !important;
}

        body.dark-theme .datepicker--cell {
            color: #e0e0e0 !important;
        }

        body.dark-theme .datepicker--cell.-weekend- {
            color: #ff9999 !important;
        }

        body.dark-theme .datepicker--cell.-other-month- {
            color: #888 !important;
        }

        body.dark-theme .datepicker--nav-title {
            color: #fff !important;
        }

        body.dark-theme .datepicker--nav-action path {
            fill: #fff !important;
        }

body.dark-theme .datepicker--cell.-selected- {
    background: #2b5278 !important;
}

body.dark-theme .datepicker--cell.-current- {
    background: #22303d !important;
}

        .datepicker--cell.-selected- {
            background: #4CAF50;
        }
		@media (max-width: 768px) {
            button:not(.close-modal):not(.btn-clear) {
                padding: 7px 15px !important;
                font-size: 12px !important;
            }
            
            body {
                padding: 5px;
                font-size: 13px;
            }

            .employee-name {
                max-width: 120px;
            }

            .action-buttons {
                flex-direction: column;
            }

            .container {
                padding: 10px;
            }

            .header {
                padding-bottom: 8px;
                margin-bottom: 8px;
            }

            .logo {
                height: 80px;
            }

            .controls button,
            .month-navigation button { 
                padding: 7px 15px; 
                font-size: 12px; 
            }

            #currentMonth {
                font-size: 1em;
            }

            th, td {
                font-size: 9px;
                padding: 4px 2px;
            }

            td:first-child,
            th:first-child { 
                min-width: 180px; 
                width: 200px; 
                max-width: 220px; 
                border-right-width: 1px; 
            }

            .employee-number {
                font-size: 0.8em;
            }

            .action-buttons button {
                font-size: 10px;
                padding: 1px 3px;
            }

            th:nth-last-child(1),
            td:nth-last-child(1),
            th:nth-last-child(2),
            td:nth-last-child(2) {
                min-width: 40px;
                width: 50px;
                font-size: 9px;
            }

            .modal-content {
                width: 90%;
                padding: 15px;
            }

            .form-group label {
                font-size: 0.9em;
            }

            .form-group input,
            .form-group select {
                padding: 7px 8px;
                font-size: 0.95em;
            }

            .modal button[type="submit"],
            #lunchModalContent button {
                padding: 7px 15px;
                font-size: 0.9em;
            }

            #lunchTable th,
            #lunchTable td {
                padding: 5px;
                font-size: 0.85em;
            }

            #lunchTable input[type="number"] {
                width: 45px;
            }

            .form-group-inline {
                flex-direction: column;
                gap: 5px;
                align-items: stretch;
            }

            .form-group-inline > div {
                flex: auto;
            }

            .form-group-inline input[type="number"] {
                width: 100%;
                flex: auto;
            }

            footer {
                font-size: 0.8em;
            }

            .footer-logo {
                height: 16px;
                width: 16px;
            }
        }

        @media (max-width: 480px) {
            button:not(.close-modal):not(.btn-clear) {
                padding: 6px 12px !important;
                font-size: 11px !important;
            }

            .employee-name {
                max-width: 90px;
            }

            .action-buttons button { 
                padding: 2px 4px !important; 
                font-size: 9px !important; 
            }

            .header {
                flex-direction: column;
                align-items: center;
            }

            .header-left {
                align-items: center;
            }

            .controls {
                justify-content: center;
                width: 100%;
                margin-top: 8px;
            }

            .logo {
                height: 70px;
            }

            .month-navigation {
                font-size: 0.9em;
            }

            .month-navigation button { 
                padding: 6px 12px; 
            }

            #currentMonth {
                margin: 0 5px;
            }

            th, td {
                font-size: 8px;
                padding: 3px 1px;
            }

            td:first-child,
            th:first-child { 
                min-width: 150px; 
                width: 170px; 
                max-width: 190px; 
            }

            .action-buttons button { 
                padding: 2px 6px !important; 
                font-size: 10px !important; 
            }

            .employee-number {
                display: none;
            }

            .action-buttons {
                gap: 2px;
            }

            .action-buttons button {
                font-size: 9px;
                padding: 1px 2px;
            }

            th:nth-last-child(1),
            td:nth-last-child(1),
            th:nth-last-child(2),
            td:nth-last-child(2) {
                min-width: 35px;
                width: 40px;
                font-size: 8px;
            }

            .modal-content {
                width: 98%;
                padding: 10px;
            }

            .form-group label {
                font-size: 0.85em;
            }

            .form-group input,
            .form-group select {
                padding: 6px 7px;
                font-size: 0.9em;
            }

            .modal button[type="submit"],
            #lunchModalContent button {
                padding: 6px 12px;
                font-size: 0.85em;
            }

            #lunchTable th,
            #lunchTable td {
                padding: 4px;
                font-size: 0.8em;
            }

            #lunchTable input[type="number"] {
                width: 40px;
            }

            footer {
                font-size: 0.75em;
                gap: 5px;
            }

            .footer-logo {
                height: 14px;
                width: 14px;
            }
        }
		@media print {
            body {
                background-color: #fff !important;
                color: #000 !important;
                -webkit-print-color-adjust: exact;
                print-color-adjust: exact;
            }

            .print-header {
                display: block !important;
                text-align: center;
                font-size: 18pt;
                font-weight: bold;
                margin: 30px 0 20px;
                line-height: 1.4;
            }

            #printMonthYear {
                font-size: 16pt;
                font-weight: normal;
                text-transform: uppercase;
            }

            .container {
                box-shadow: none;
                border: none;
                padding: 0;
                margin: 0;
                max-width: 100%;
                flex-grow: 0;
                background-color: #fff !important;
            }

            .no-print {
                display: none !important;
            }

            .header {
                border: none;
                margin-bottom: 5px;
            }

            .logo {
                display: none;
            }

            #calendar {
                overflow: visible;
            }

            table {
                table-layout: auto;
                width: 100%;
                min-width: auto;
                border-color: #000 !important;
            }

            th, td {
                color: #000 !important;
                border: 1px solid #000 !important;
                background-color: #fff !important;
                -webkit-print-color-adjust: exact;
                print-color-adjust: exact;
            }

            th {
                background-color: #f2f2f2 !important;
            }

            td:first-child,
            th:first-child {
                position: static;
                background-color: #fff !important;
                min-width: auto;
                width: auto;
                max-width: none;
                border-right: 2px solid #000 !important;
            }

            .employee-info {
                color: #000 !important;
            }

            .weekend,
            .vacation,
            .sick {
                background-color: inherit !important;
            }

            .vacation::after {
                content: "";
                font-size: 0.8em;
            }

            .sick::after {
                content: "";
                font-size: 0.8em;
            }

            .print-footer {
                display: block !important;
                margin-top: 30px;
                text-align: right;
                color: #000 !important;
            }

            footer {
                display: none;
            }

            .weekend { 
                background-color: #dedede !important; 
                -webkit-print-color-adjust: exact;
                print-color-adjust: exact;
            }

            .vacation, .sick { 
                background-color: inherit !important; 
            }
        }

        /* Стили для списка сокращенных смен */
        #shortenedList {
            border: 1px solid #ddd;
            border-radius: 4px;
            padding: 10px;
        }

        .shortened-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 8px;
            margin: 5px 0;
            background-color: #f5f5f5;
            border-radius: 4px;
        }

        .shortened-item button {
            margin-left: 10px;
            padding: 3px 8px !important;
        }

body.dark-theme .shortened-item {
    background-color: #22303d;
    border-color: #2b5278;
}

        .shortened-display-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin: 3px 0;
        }
    </style>
</head>
<body>
    <audio id="heartSound" src="heart.mp3" loop></audio>
    <audio id="audioPlayer" src="pomogator.mp3"></audio>

    <div class="container">
        <div class="print-header" style="display: none;">
            ГРАФИК СЛЕСАРЕЙ-РЕМОНТНИКОВ РМЦ, УЧАСТОК ПО РЕМОНТУ ОБОРУДОВАНИЯ ЛРП<br>
            (<span id="printMonthYear"></span>)
        </div>

        <div class="header">
            <div class="header-left">
                <img src="img/logo1.png" class="logo" alt="Логотип" 
                     onerror="this.src='https://placehold.co/150x60/cccccc/333333?text=Logo+Error'; this.onerror=null;"
                     onmouseenter="document.getElementById('heartSound').play()"
                     onmouseleave="document.getElementById('heartSound').pause()">
                <h1 class="header-title no-print" onclick="document.getElementById('audioPlayer').play()">Помогатор V3.3</h1>
                <div class="controls no-print">
                    <button onclick="openEditModal(-1)">➕ Добавить</button>
                    <button onclick="exportToFile()">💾 Сохранить</button>
                    <button onclick="document.getElementById('fileInput').click()">📂 Загрузить</button>
                    <button onclick="exportTable()">📊 Excel (.xlsx)</button>
                    <button onclick="window.print()">🖨 Печать</button>
                    <button onclick="toggleTheme()">🌓 Тема</button>
                    <button id="lunchSettingsBtn">🍽️ Обед</button>
                </div>
            </div>
        </div>

        <div class="month-navigation no-print">
            <button onclick="changeMonth(-1)">← Предыдущий</button>
            <div id="currentMonth"></div>
            <button onclick="changeMonth(1)">Следующий →</button>
        </div>

        <div id="calendar"></div>

        <div class="print-footer" style="display: none;">
            <p>График утвердил: _______________________________________</p>
        </div>
    </div>

    <!-- Модальные окна -->
    <div id="editModal" class="modal">
        <div class="modal-content">
            <span class="close-modal" onclick="closeModal('editModal')">&times;</span>
            <h3>Управление работником</h3>
            <form onsubmit="handleEmployeeForm(event)">
                <div class="form-group">
                    <label>Табельный номер:</label>
                    <input type="text" id="employeeNumber" required>
                </div>
                <div class="form-group">
                    <label>ФИО:</label>
                    <input type="text" id="employeeName" required>
                </div>
                <div class="form-group">
                    <label>График работы:</label>
                    <select id="scheduleType" required>
                        <option value="5days">5/2 Дневной (8:00-17:00)</option>
                        <option value="2/2">Смены 2/2 (2Д/2В)</option>
                        <option value="2/2/4">Смены 2Д/2Н/4В</option>
                        <option value="3/3">3/3 (3 рабочих/3 выходных)</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>Дата начала работы:</label>
                    <input type="date" id="startDate" required>
                </div>
                <div class="form-group">
                    <label>Отпуск (начало - дней):</label>
                    <div class="form-group-inline">
                        <div><input type="date" id="vacationStart"></div>
                        <div><input type="number" id="vacationDays" min="1" placeholder="Дней"></div>
                    </div>
                </div>
                <div class="form-group">
                    <label>Больничный (начало - конец):</label>
                    <div class="form-group-inline">
                        <div><input type="date" id="sickStart"></div>
                        <div><input type="date" id="sickEnd"></div>
                    </div>
                </div>
                <div class="form-group">
                    <label>Выходные дни:
                        <button type="button" onclick="openDatepicker()" style="margin-left:10px;">📅 Выбрать даты</button>
                    </label>
                    <input type="text" id="customDaysOff" placeholder="ДД.ММ.ГГГГ через запятую">
                </div>
                <div class="form-group">
                    <label>Не стандартные смены (часы):
                       <button type="button" onclick="openShortenedModal()" 
        style="margin-left:10px;"
        ${currentEmployeeIndex === -1 ? 'disabled' : ''}>
    ⏱️ Выбрать даты
</button>
                    </label>
                    <div id="shortenedShiftsDisplay" style="margin-top:5px; font-size:0.9em;"></div>
                </div>
                <div class="action-buttons">
                    <button type="submit">Сохранить</button>
                </div>
            </form>
        </div>
    </div>

    <div id="lunchModal" class="modal">
        <div class="modal-content" id="lunchModalContent">
            <span class="close-modal" onclick="closeModal('lunchModal')">&times;</span>
            <h3>Настройки обеденного времени</h3>
            <table id="lunchTable">
                <thead>
                    <tr>
                        <th>График</th>
                        <th>Тип смены</th>
                        <th>Обед 1, мин</th>
                        <th>Обед 2, мин</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>5/2 Дневной</td>
                        <td>Дневная</td>
                        <td><input type="number" value="60" data-schedule="5days-day-1" class="lunch-time-input"></td>
                        <td><input type="number" value="0" data-schedule="5days-day-2" class="lunch-time-input"></td>
                    </tr>
                    <tr>
                        <td>Смены 2/2</td>
                        <td>Дневная</td>
                        <td><input type="number" value="30" data-schedule="2/2-day-1" class="lunch-time-input"></td>
                        <td><input type="number" value="0" data-schedule="2/2-day-2" class="lunch-time-input"></td>
                    </tr>
                    <tr>
                        <td>Смены 2/2/4</td>
                        <td>Дневная</td>
                        <td><input type="number" value="30" data-schedule="2/2/4-day-1" class="lunch-time-input"></td>
                        <td><input type="number" value="0" data-schedule="2/2/4-day-2" class="lunch-time-input"></td>
                    </tr>
                    <tr>
                        <td>Смены 2/2/4</td>
                        <td>Ночная</td>
                        <td><input type="number" value="30" data-schedule="2/2/4-night-1" class="lunch-time-input"></td>
                        <td><input type="number" value="0" data-schedule="2/2/4-night-2" class="lunch-time-input"></td>
                    </tr>
                    <tr>
                        <td>3/3</td>
                        <td>Дневная</td>
                        <td><input type="number" value="30" data-schedule="3/3-day-1" class="lunch-time-input"></td>
                        <td><input type="number" value="0" data-schedule="3/3-day-2" class="lunch-time-input"></td>
                    </tr>
                </tbody>
            </table>
            <button onclick="saveLunchSettings()">Сохранить настройки</button>
        </div>
    </div>

    <div id="datepickerModal" class="modal">
        <div class="modal-content" style="max-width: 320px;">
            <span class="close-modal" onclick="closeModal('datepickerModal')">&times;</span>
            <h3 style="margin-bottom: 15px;">Выберите выходные дни</h3>
            <div id="datepicker"></div>
            <div style="margin-top: 15px;">
                <button onclick="addSelectedDates()" style="padding: 8px 15px; margin-right: 10px;">Сохранить</button>
                <button onclick="clearSelectedDates()" class="btn-clear">Очистить</button>
            </div>
        </div>
    </div>

<div id="shortenedModal" class="modal">
    <div class="modal-content" style="max-width: 500px;">
        <span class="close-modal" onclick="closeModal('shortenedModal')">&times;</span>
        <h3>Сокращенные смены</h3>
        
        <!-- Форма для добавления/редактирования -->
        <div class="form-group">
            <label>Дата:</label>
            <input type="date" id="shortenedDate" class="shortened-input">
        </div>
        <div class="form-group">
            <label>Часы работы:</label>
            <input type="number" id="shortenedHours" min="0" max="12" step="0.5" value="7" 
                   class="shortened-input">
        </div>
        <button onclick="addShortenedShift()" id="addShortenedBtn">➕ Добавить</button>
        
        <!-- Список существующих записей -->
        <div id="shortenedList" style="margin-top: 15px; max-height: 300px; overflow-y: auto;">
            <!-- Записи будут добавляться здесь -->
        </div>
        
        <div style="margin-top: 15px; display: flex; gap: 10px;">
            <button onclick="closeModal('shortenedModal')">Закрыть</button>
        </div>
    </div>
</div>

    <input type="file" id="fileInput" hidden accept=".json">

    <footer class="no-print">
        <img src="img/logo.png" class="footer-logo" alt="Логотип">
        © NeBOLTaY Трыц-Тыц Помогатор, 2025
    </footer>

    <script>
        let currentDate = new Date();
        let employees = [];
        let currentEmployeeIndex = null;
        let lunchSettings = {};
        const monthNames = ["Январь", "Февраль", "Март", "Апрель", "Май", "Июнь", 
                          "Июль", "Август", "Сентябрь", "Октябрь", "Ноябрь", "Декабрь"];
        let selectedDates = [];
        let currentDateInput = null;
        let currentShortenedEmployee = null;
        let currentShortenedDates = [];

        // Функция для преобразования данных из старых версий
        function migrateData(data) {
            if (!data.version) {
                console.log("Миграция данных из версии 1");
                return {
                    version: 3,
                    employees: data.map(emp => ({
                        number: emp.number || '',
                        name: emp.name,
                        scheduleType: emp.scheduleType || '5days',
                        startDate: emp.startDate || new Date().toISOString(),
                        vacation: emp.vacation || [],
                        sickLeaves: emp.sickLeaves || [],
                        customDaysOff: emp.customDaysOff || [],
                        shortenedShifts: {}
                    })),
                    lunchSettings: {}
                };
            }
            if (data.version === 2) {
                console.log("Миграция данных из версии 2");
                return {
                    version: 3,
                    employees: data.employees,
                    lunchSettings: data.lunchSettings || {}
                };
            }
            return data;
        }

        // Функция для проверки корректности данных работника
        function validateEmployee(emp) {
            if (!emp || typeof emp !== 'object') {
                console.error("Некорректный объект работника", emp);
                return false;
            }
            
            const requiredFields = ['name', 'scheduleType', 'startDate'];
            for (const field of requiredFields) {
                if (!emp[field]) {
                    console.error(`У работника отсутствует обязательное поле: ${field}`, emp);
                    return false;
                }
            }
            
            if (isNaN(new Date(emp.startDate).getTime())) {
                console.error(`Некорректная дата начала работы у работника ${emp.name}`, emp);
                return false;
            }
            
            return true;
        }

        class Employee {
constructor({ 
    number, 
    name, 
    scheduleType, 
    startDate, 
    vacation = [], 
    sickLeaves = [], 
    customDaysOff = [],
    shortenedShifts = {},
    carryOverHours = {}  // Добавили новое поле
}) {
                // Проверка обязательных полей
                if (!name) throw new Error("Не указано имя работника");
                
                this.number = number || '';
                this.name = name;
                this.scheduleType = scheduleType || '5days';
                
                // Обработка даты начала работы
                try {
                    this.startDate = new Date(startDate);
                    if (isNaN(this.startDate.getTime())) {
                        console.warn(`Некорректная дата начала для ${name}, установлена текущая дата`);
                        this.startDate = new Date();
                    }
                } catch (e) {
                    console.warn(`Ошибка в дате начала для ${name}, установлена текущая дата`, e);
                    this.startDate = new Date();
                }
                this.startDate.setUTCHours(0, 0, 0, 0);
                
                // Обработка отпусков
this.vacation = vacation.map(p => ({
    start: this.parseDate(p.start),
    end: p.end ? this.parseDate(p.end) : this.parseDate(p.start)
})).filter(p => p.start);

// Обработка больничных
this.sickLeaves = sickLeaves.map(p => ({
    start: this.parseDate(p.start),
    end: p.end ? this.parseDate(p.end) : this.parseDate(p.start)
})).filter(p => p.start);
                
                // Обработка выходных
                this.customDaysOff = customDaysOff
                    .map(d => this.parseDate(d))
                    .filter(d => d)
                    .map(d => d.toISOString().split('T')[0]);
                
                // Обработка сокращенных смен
this.shortenedShifts = {};
Object.entries(shortenedShifts || {}).forEach(([date, hours]) => {
    const parsedDate = this.parseDate(date);
    if (parsedDate) {
        this.shortenedShifts[parsedDate.toISOString().split('T')[0]] = hours;
    }
});

// Добавили инициализацию переносимых часов
this.carryOverHours = carryOverHours;
            }
getMonthKey(date) {
    const d = new Date(date);
    return `${d.getUTCFullYear()}-${d.getUTCMonth()}`;
}

getNextMonthKey(date) {
    const d = new Date(date);
    d.setUTCMonth(d.getUTCMonth() + 1);
    return `${d.getUTCFullYear()}-${d.getUTCMonth()}`;
}

isLastDayOfMonth(date) {
    const d = new Date(date);
    return d.getUTCDate() === new Date(d.getUTCFullYear(), d.getUTCMonth() + 1, 0).getUTCDate();
}
            // Вспомогательный метод для парсинга дат
parseDate(dateStr) {
    if (!dateStr) return null;
    
    // Парсинг ISO формата "2024-05-01"
    const isoMatch = dateStr.match(/^(\d{4})-(\d{2})-(\d{2})/);
    if (isoMatch) {
        return new Date(Date.UTC(isoMatch[1], isoMatch[2]-1, isoMatch[3]));
    }
    
    // Парсинг формата "01.05.2024"
    const dmyMatch = dateStr.match(/^(\d{2})\.(\d{2})\.(\d{4})/);
    if (dmyMatch) {
        return new Date(Date.UTC(dmyMatch[3], dmyMatch[2]-1, dmyMatch[1]));
    }
    
    // Парсинг других форматов
    try {
        const date = new Date(dateStr);
        if (!isNaN(date.getTime())) {
            date.setUTCHours(0, 0, 0, 0);
            return date;
        }
    } catch (e) {
        console.warn("Не удалось распарсить дату:", dateStr, e);
    }
    
    return null;
}

            isCustomDayOff(date) {
                const checkDate = new Date(date);
                checkDate.setHours(0, 0, 0, 0);
                return this.customDaysOff.some(d => {
                    const storedDate = new Date(d);
                    storedDate.setHours(0, 0, 0, 0);
                    return storedDate.getTime() === checkDate.getTime();
                });
            }

isVacation(date) {
    const checkDate = new Date(date);
    checkDate.setUTCHours(0, 0, 0, 0);
    
    return this.vacation.some(p => {
        const start = new Date(p.start);
        start.setUTCHours(0, 0, 0, 0);
        
        let end = p.end ? new Date(p.end) : start;
        end.setUTCHours(0, 0, 0, 0);
        
        return checkDate >= start && checkDate <= end;
    });
}

isSick(date) {
    const checkDate = new Date(date);
    checkDate.setUTCHours(0, 0, 0, 0);
    
    return this.sickLeaves.some(p => {
        const start = new Date(p.start);
        start.setUTCHours(0, 0, 0, 0);
        
        let end = p.end ? new Date(p.end) : start;
        end.setUTCHours(0, 0, 0, 0);
        
        return checkDate >= start && checkDate <= end;
    });
}

            getShift(date) {
    try {
        const checkDate = new Date(date);
        if (isNaN(checkDate.getTime())) {
            console.warn("Некорректная дата:", date);
            return '';
        }
        
        checkDate.setUTCHours(0, 0, 0, 0);
        const checkDateStr = checkDate.toISOString().split('T')[0];
        
        // 1. Проверяем сокращенные смены
        if (this.shortenedShifts[checkDateStr] !== undefined) {
            return this.shortenedShifts[checkDateStr];
        }
        
        // 2. Проверяем выходные
        if (this.isCustomDayOff(checkDateStr)) return '';
        
        // 3. Проверяем больничный
        if (this.isSick(checkDateStr)) return 'Б';
        
        // 4. Проверяем отпуск
        if (this.isVacation(checkDateStr)) return 'О';
        
        // 5. Проверяем что дата не раньше начала работы
        const startDate = new Date(this.startDate);
        startDate.setUTCHours(0, 0, 0, 0);
        if (checkDate < startDate) return '';
        
        // 6. Определяем смену по графику
        const diffTime = checkDate - startDate;
        const diffDays = Math.floor(diffTime / (1000 * 60 * 60 * 24));
        
        switch (this.scheduleType) {
            case '5days':
                return (diffDays % 7 < 5) ? '8' : '';
            case '2/2':
                return (diffDays % 4 < 2) ? 'Д' : '';
            case '2/2/4':
                const cycle = diffDays % 8;
                return cycle < 2 ? 'Д' : cycle < 4 ? 'Н' : '';
            case '3/3':
                return (diffDays % 6 < 3) ? 'Д' : '';
            default:
                return '';
        }
    } catch (e) {
        console.error("Ошибка в getShift:", e);
        return '';
    }
}

getHours(date) {
    const checkDate = new Date(date).toISOString().split('T')[0];
    
    // Добавили обработку переносимых часов
    const monthKey = this.getMonthKey(date);
    let carryOver = this.carryOverHours[monthKey] || 0;
    delete this.carryOverHours[monthKey];

    // Для сокращенных смен возвращаем значение как есть
    if (this.shortenedShifts[checkDate] !== undefined) {
        return this.shortenedShifts[checkDate] + carryOver;
    }
    
    const shift = this.getShift(date);
    if (!shift || shift === 'Б' || shift === 'О') return 0;
    
    // Для обычных смен продолжаем расчет
    let baseHours = 0;
    let shiftType = 'day';
    
    switch (this.scheduleType) {
        case '5days':
            baseHours = 9;
            shiftType = 'day';
            break;
        case '2/2':
            baseHours = 12;
            shiftType = 'day';
            break;
        case '2/2/4':
            baseHours = 12;
            shiftType = shift === 'Н' ? 'night' : 'day';
            break;
        case '3/3':
            baseHours = 12;
            shiftType = 'day';
            break;
    }

const lunch1 = lunchSettings[`${this.scheduleType}-${shiftType}-1`] || 0;
    const lunch2 = lunchSettings[`${this.scheduleType}-${shiftType}-2`] || 0;
    let totalHours = baseHours - (lunch1 + lunch2)/60;

    // Добавили обработку последнего дня месяца для ночных смен
    if (shift === 'Н' && this.isLastDayOfMonth(date)) {
        const nextMonthKey = this.getNextMonthKey(date);
        const hoursForCurrentMonth = 4;
        const hoursToCarry = totalHours - hoursForCurrentMonth;
        
        this.carryOverHours[nextMonthKey] = (this.carryOverHours[nextMonthKey] || 0) + hoursToCarry;
        totalHours = hoursForCurrentMonth;
    }

    return totalHours + carryOver;
}
        }

        function generateCalendar() {
            const month = currentDate.getUTCMonth();
            const year = currentDate.getUTCFullYear();
            
            document.getElementById('printMonthYear').textContent = 
                `${monthNames[month]} ${year} г.`.toUpperCase();
            
            const calendarDiv = document.getElementById('calendar');
            calendarDiv.innerHTML = '';
            
            const daysInMonth = new Date(Date.UTC(year, month + 1, 0)).getUTCDate();
            
            document.getElementById('currentMonth').textContent = `${monthNames[month]} ${year}`.toUpperCase();
            
            const table = document.createElement('table');
            const thead = table.createTHead();
            const tbody = table.createTBody();
            
            const headerRow = thead.insertRow();
            headerRow.innerHTML = '<th>Работник</th>' + 
                Array.from({length: daysInMonth}, (_, i) => {
                    const date = new Date(Date.UTC(year, month, i + 1));
                    const isWeekend = [0,6].includes(date.getUTCDay());
                    return `<th class="${isWeekend ? 'weekend' : ''}">${i + 1}</th>`;
                }).join('') + 
                '<th>Всего смен</th><th>Итого часов</th>';

            employees.sort((a, b) => (a.number || '').localeCompare(b.number || '', undefined, {numeric: true}));

            employees.forEach((employee, index) => {
                const row = tbody.insertRow();
                const nameCell = row.insertCell();
                nameCell.innerHTML = `
                    <div class="employee-info">
                        <span class="employee-name" title="Табельный номер: ${employee.number || 'N/A'}">
                            ${employee.name}
                            <span class="employee-number">№${employee.number || 'N/A'}</span>
                        </span>
                        <div class="action-buttons no-print">
                            <button class="btn-edit" onclick="openEditModal(${index})" title="Редактировать">✏️</button>
                            <button class="btn-delete" onclick="deleteEmployee(${index})" title="Удалить">🗑️</button>
                        </div>
                    </div>
                `;

                let totalShifts = 0;
                let totalHours = 0;

                for (let day = 1; day <= daysInMonth; day++) {
                    const date = new Date(Date.UTC(year, month, day));
                    const cell = row.insertCell();
                    const shift = employee.getShift(date);
                    cell.textContent = shift;

                    if (typeof shift === 'number') {
                        cell.classList.add('shortened');
                        cell.title = `Сокращенная смена: ${shift} часов`;
                    }

                    if (shift && shift !== 'Б' && shift !== 'О') {
                        totalShifts++;
                        totalHours += employee.getHours(date);
                    }

                    if (employee.isVacation(date)) cell.classList.add('vacation');
                    if (employee.isSick(date)) cell.classList.add('sick');
                    const isWeekend = [0,6].includes(date.getUTCDay());
                    if (isWeekend) cell.classList.add('weekend');
                }

                row.insertCell().textContent = totalShifts;
                // Добавить перед вставкой ячейки с часами
const prevMonthDate = new Date(Date.UTC(year, month, 1));
prevMonthDate.setUTCMonth(prevMonthDate.getUTCMonth() - 1);
const prevMonthKey = employee.getMonthKey(prevMonthDate);
totalHours += employee.carryOverHours[prevMonthKey] || 0;

row.insertCell().textContent = totalHours.toFixed(1);
            });

            calendarDiv.appendChild(table);
        }

        function changeMonth(offset) {
            currentDate.setMonth(currentDate.getMonth() + offset);
            generateCalendar();
        }

        function openEditModal(index = -1) {
    currentShortenedEmployee = index !== -1 ? employees[index] : null; // Устанавливаем текущего сотрудника
    try {
        currentEmployeeIndex = index;
        const modal = document.getElementById('editModal');
        if (!modal) {
            console.error('Модальное окно editModal не найдено');
            return;
        }
                
                const form = modal.querySelector('form');
                if (!form) {
                    console.error('Форма в модальном окне не найдена');
                    return;
                }
                
                form.reset();

                if (index === -1) {
                    modal.querySelector('h3').textContent = 'Добавить работника';
                } else {
                    modal.querySelector('h3').textContent = 'Редактирование работника';
                    const emp = employees[index];
                    
                    document.getElementById('employeeNumber').value = emp.number;
                    document.getElementById('employeeName').value = emp.name;
                    document.getElementById('scheduleType').value = emp.scheduleType;
                    document.getElementById('startDate').value = emp.startDate.toISOString().split('T')[0];
                    
                    if (emp.vacation.length > 0) {
    const vacation = emp.vacation[0];
    // Преобразуем дату в формат YYYY-MM-DD для input[type=date]
    const startDate = new Date(vacation.start);
    const startDateISO = startDate.toISOString().split('T')[0];
    document.getElementById('vacationStart').value = startDateISO;
    
    if (vacation.end) {
        const endDate = new Date(vacation.end);
        const diffDays = Math.ceil((endDate - startDate) / (1000 * 60 * 60 * 24)) + 1;
        document.getElementById('vacationDays').value = diffDays;
    }
}
                    
if (emp.sickLeaves.length > 0) {
    const sickLeave = emp.sickLeaves[0];
    const start = new Date(sickLeave.start);
    const end = new Date(sickLeave.end);
    document.getElementById('sickStart').value = start.toISOString().split('T')[0];
    document.getElementById('sickEnd').value = end.toISOString().split('T')[0];
}
                    
                    document.getElementById('customDaysOff').value = emp.customDaysOff
                        .map(d => {
                            const date = new Date(d);
                            return `${String(date.getUTCDate()).padStart(2, '0')}.${String(date.getUTCMonth() + 1).padStart(2, '0')}.${date.getUTCFullYear()}`;
                        }).join(', ');

                    const displayDiv = document.getElementById('shortenedShiftsDisplay');
if (displayDiv) {
    displayDiv.innerHTML = Object.entries(emp.shortenedShifts)
        .map(([date, hours]) => {
            const utcDate = new Date(date + 'T00:00:00Z');
            const localDate = new Date(utcDate.getTime() - utcDate.getTimezoneOffset() * 60000);
            const day = localDate.getDate().toString().padStart(2, '0');
            const month = (localDate.getMonth() + 1).toString().padStart(2, '0');
            const year = localDate.getFullYear();
            return `
                <div class="shortened-display-item">
                    ${day}.${month}.${year}: ${hours}ч
                    <button onclick="deleteShortenedDisplay('${date}')" class="no-print">🗑️</button>
                </div>
            `;
        }).join('');
}
                }

                modal.style.display = 'flex';
            } catch (error) {
                console.error('Ошибка в openEditModal:', error);
            }
        }
function deleteShortenedDisplay(dateKey) {
    if (!currentShortenedEmployee) {
        console.error("Не выбран сотрудник для удаления");
        return;
    }
  if (confirm('Удалить эту запись?')) {
        delete currentShortenedEmployee.shortenedShifts[dateKey];
        openEditModal(currentEmployeeIndex); // Переоткрываем окно для обновления
        saveData();
        generateCalendar();
    }
}
function handleEmployeeForm(e) {
    e.preventDefault();
    
    const formData = {
        number: document.getElementById('employeeNumber').value.trim(),
        name: document.getElementById('employeeName').value.trim(),
        scheduleType: document.getElementById('scheduleType').value,
        startDate: document.getElementById('startDate').value,
        vacation: [],
        sickLeaves: [],
        customDaysOff: document.getElementById('customDaysOff').value.split(',').map(d => d.trim()).filter(d => d),
        shortenedShifts: currentShortenedEmployee ? currentShortenedEmployee.shortenedShifts : {}
    };

            const vacationStart = document.getElementById('vacationStart').value;
            const vacationDays = parseInt(document.getElementById('vacationDays').value, 10);
            if (vacationStart && vacationDays > 0) {
			if (isNaN(new Date(vacationStart).getTime())) {
        alert("Некорректная дата начала отпуска");
        return;
    }
                if (vacationStart && vacationDays > 0) {
    const startDate = new Date(vacationStart);
    startDate.setUTCHours(0, 0, 0, 0);
    const endDate = new Date(startDate);
    endDate.setUTCDate(startDate.getUTCDate() + vacationDays - 1);
    
    formData.vacation.push({
        start: startDate.toISOString(),
        end: endDate.toISOString()
    });
}}

            const sickStart = document.getElementById('sickStart').value;
            const sickEnd = document.getElementById('sickEnd').value;
            if (sickStart && sickEnd) {
			if (isNaN(new Date(sickStart).getTime()) || isNaN(new Date(sickEnd).getTime())) {
        alert("Некорректные даты больничного");
        return;
    }
               if (sickStart && sickEnd) {
    const start = new Date(sickStart);
    start.setUTCHours(0, 0, 0, 0);
    const end = new Date(sickEnd);
    end.setUTCHours(0, 0, 0, 0);
    
    if (end >= start) {
        formData.sickLeaves.push({
            start: start.toISOString(),
            end: end.toISOString()
        });
    }
}}

            const newEmployee = new Employee(formData);
            
            if (currentEmployeeIndex === -1) {
                employees.push(newEmployee);
            } else {
                employees[currentEmployeeIndex] = newEmployee;
            }

            saveData();
            generateCalendar();
            closeModal('editModal');
        }

        function deleteEmployee(index) {
            if (confirm(`Удалить работника "${employees[index].name}"?`)) {
                employees.splice(index, 1);
                saveData();
                generateCalendar();
            }
        }

        function exportToFile() {
            const data = {
                version: 3,
employees: employees.map(emp => ({
    number: emp.number,
    name: emp.name,
    scheduleType: emp.scheduleType,
    startDate: emp.startDate.toISOString(),
    vacation: emp.vacation,
    sickLeaves: emp.sickLeaves,
    customDaysOff: emp.customDaysOff,
    shortenedShifts: emp.shortenedShifts,
    carryOverHours: emp.carryOverHours  // Добавили новое поле
})),
                lunchSettings: lunchSettings
            };

            const blob = new Blob([JSON.stringify(data, null, 2)], {type: 'application/json'});
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = 'schedule_data.json';
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
        }

        function exportTable() {
            try {
                const originalTable = document.querySelector('#calendar table');
                if (!originalTable) throw new Error('Таблица не найдена');

                const clonedTable = originalTable.cloneNode(true);
                clonedTable.querySelectorAll('.no-print, .employee-number, .action-buttons').forEach(el => el.remove());
                
                clonedTable.querySelectorAll('.employee-name').forEach(el => {
                    el.parentElement.innerHTML = el.textContent;
                });

                const ws = XLSX.utils.table_to_sheet(clonedTable);

                const headerRow = originalTable.tHead?.rows[0];
                const weekendColumns = [];
                if (headerRow) {
                    Array.from(headerRow.cells).forEach((cell, index) => {
                        if (cell.classList.contains('weekend')) weekendColumns.push(index);
                    });
                }

                const range = XLSX.utils.decode_range(ws['!ref']);
                for (let R = range.s.r; R <= range.e.r; ++R) {
                    for (let C = range.s.c; C <= range.e.c; ++C) {
                        const cellAddress = XLSX.utils.encode_cell({ r: R, c: C });
                        if (!ws[cellAddress]) continue;

                        ws[cellAddress].s = {
                            alignment: {
                                horizontal: C === 0 ? 'left' : 'center',
                                vertical: 'center',
                                wrapText: true
                            },
                            font: { sz: 11 }
                        };

                        if (weekendColumns.includes(C)) {
                            ws[cellAddress].s.fill = {
                                patternType: "solid",
                                fgColor: { theme: 8, tint: 0.4 }
                            };
                        }

                        if (R === 0) {
                            ws[cellAddress].s.font.bold = true;
                        }
                    }
                }

                if (clonedTable.rows.length > 0) {
                    const firstRow = clonedTable.rows[0];
                    ws['!cols'] = Array.from(firstRow.cells).map((cell, index) => ({
                        wch: index === 0 ? 35 : 5
                    }));
                }

                const wb = XLSX.utils.book_new();
                XLSX.utils.book_append_sheet(wb, ws, "График смен");
                XLSX.writeFile(wb, `График_смен_${new Date().toISOString().slice(0,10)}.xlsx`);

            } catch (error) {
                console.error('Ошибка экспорта:', error);
                alert('Ошибка экспорта: ' + error.message);
            }
        }

        function toggleTheme() {
            document.body.classList.toggle('dark-theme');
            localStorage.setItem('theme', document.body.classList.contains('dark-theme') ? 'dark' : 'light');
        }

        function closeModal(modalId) {
            document.getElementById(modalId).style.display = 'none';
        }

        function saveData() {
            const data = {
                version: 3,
                employees: employees
                    .filter(emp => {
                        if (!validateEmployee(emp)) {
                            console.error("Некорректный работник не будет сохранен:", emp);
                            return false;
                        }
                        return true;
                    })
                    .map(emp => ({
                        number: emp.number,
                        name: emp.name,
                        scheduleType: emp.scheduleType,
                        startDate: emp.startDate.toISOString(),
                        vacation: emp.vacation,
                        sickLeaves: emp.sickLeaves,
                        customDaysOff: emp.customDaysOff,
                        shortenedShifts: emp.shortenedShifts
                    })),
                lunchSettings: lunchSettings
            };
            
            try {
                localStorage.setItem('scheduleAppData', JSON.stringify(data));
                console.log("Данные успешно сохранены");
            } catch (e) {
                console.error("Ошибка сохранения:", e);
                alert("Не удалось сохранить данные: " + e.message);
            }
        }

function saveLunchSettings() {
    document.querySelectorAll('#lunchTable input').forEach(input => {
        const key = input.dataset.schedule;
        const value = parseInt(input.value, 10) || 0;
        lunchSettings[key] = value;
    });
    saveData();
    generateCalendar(); // Добавить эту строку
    closeModal('lunchModal');
}
let currentEditingShift = null;

function openShortenedModal() {
	// Проверка корректности индекса сотрудника
    if (currentEmployeeIndex === null || currentEmployeeIndex < 0 || currentEmployeeIndex >= employees.length) {
        alert("Сначала выберите или создайте сотрудника");
        return;
    }
    currentShortenedEmployee = employees[currentEmployeeIndex];
    refreshShortenedList();
    document.getElementById('shortenedModal').style.display = 'flex';
}

function refreshShortenedList() {
    const list = document.getElementById('shortenedList');
    list.innerHTML = '';
	if (!currentShortenedEmployee) {
        console.error("Не выбран сотрудник для редактирования");
        return;
    }
    
    Object.entries(currentShortenedEmployee.shortenedShifts).forEach(([date, hours]) => {
        const item = document.createElement('div');
        item.className = 'shortened-item';
        
        // Конвертация UTC даты в локальный формат
        const utcDate = new Date(date + 'T00:00:00Z');
        const localDate = new Date(utcDate.getTime() - utcDate.getTimezoneOffset() * 60000);
        const dateStr = localDate.toLocaleDateString('ru-RU');
        
        item.innerHTML = `
            <span>${dateStr} - ${hours} ч</span>
            <div>
                <button onclick="editShortenedShift('${date}')">✏️</button>
                <button onclick="deleteShortenedShift('${date}')">🗑️</button>
            </div>
        `;
        
        list.appendChild(item);
    });
}

function addShortenedShift() {
    const dateInput = document.getElementById('shortenedDate');
    const hoursInput = document.getElementById('shortenedHours');
    
    const date = new Date(dateInput.value);
    if (isNaN(date.getTime())) {
        alert('Выберите корректную дату');
        return;
    }
    
    // Конвертация в UTC
    const utcDate = new Date(Date.UTC(
        date.getFullYear(),
        date.getMonth(),
        date.getDate()
    ));
    const isoDate = utcDate.toISOString().split('T')[0];
    
    if (currentEditingShift) {
        // Удаляем старую запись при редактировании
        delete currentShortenedEmployee.shortenedShifts[currentEditingShift];
        currentEditingShift = null;
    }
    
    currentShortenedEmployee.shortenedShifts[isoDate] = parseFloat(hoursInput.value);
    
    // Сброс формы
    dateInput.value = '';
    hoursInput.value = '7';
    document.getElementById('addShortenedBtn').textContent = '➕ Добавить';
    
    refreshShortenedList();
    saveData();
    generateCalendar();
}

function editShortenedShift(dateKey) {
    const hours = currentShortenedEmployee.shortenedShifts[dateKey];
    
    // Конвертация UTC в локальную дату для input
    const utcDate = new Date(dateKey + 'T00:00:00Z');
    const localDate = new Date(utcDate.getTime() - utcDate.getTimezoneOffset() * 60000);
    const formattedDate = localDate.toISOString().split('T')[0];
    
    document.getElementById('shortenedDate').value = formattedDate;
    document.getElementById('shortenedHours').value = hours;
    currentEditingShift = dateKey;
    
    document.getElementById('addShortenedBtn').textContent = '💾 Сохранить';
}

function deleteShortenedShift(dateKey) {
    if (confirm('Удалить эту запись?')) {
        delete currentShortenedEmployee.shortenedShifts[dateKey];
        refreshShortenedList();
        saveData();
        generateCalendar();
    }
}

        function openDatepicker() {
            currentDateInput = document.getElementById('customDaysOff');
            
selectedDates = currentDateInput.value.split(',')
    .map(d => {
        const parts = d.trim().match(/(\d{2})\.(\d{2})\.(\d{4})/);
        if (!parts) return null;
        // Создаем дату в UTC 
        return new Date(Date.UTC(parts[3], parts[2]-1, parts[1]));
    })
    .filter(d => d && !isNaN(d));

            const $datepicker = $('#datepicker');
            if ($datepicker.data('datepicker')) {
                $datepicker.data('datepicker').destroy();
            }
            
            $datepicker.datepicker({
                language: 'ru',
                dateFormat: 'dd.mm.yyyy',
                multipleDates: true,
                toggleSelected: false,
                selectedDates: selectedDates,
                onSelect: function(formattedDate, date) {
                    selectedDates = Array.isArray(date) ? date : [date];
                }
            });

            document.getElementById('datepickerModal').style.display = 'flex';
        }

        function addSelectedDates() {
            if (currentDateInput && selectedDates.length > 0) {
                const formattedDates = selectedDates
                    .map(d => {
                        const day = String(d.getDate()).padStart(2, '0');
                        const month = String(d.getMonth() + 1).padStart(2, '0');
                        const year = d.getFullYear();
                        return `${day}.${month}.${year}`;
                    });
                currentDateInput.value = formattedDates.join(', ');
            } else {
                currentDateInput.value = '';
            }
            closeModal('datepickerModal');
        }

        function clearSelectedDates() {
            selectedDates = [];
            currentDateInput.value = '';
            $('#datepicker').datepicker().data('datepicker').clear();
            closeModal('datepickerModal');
        }


        document.addEventListener('DOMContentLoaded', () => {
            const savedData = localStorage.getItem('scheduleAppData');
            if (savedData) {
                try {
                    console.log("Загрузка сохраненных данных...");
                    const data = JSON.parse(savedData);
                    const migratedData = migrateData(data);
                    console.log("Данные после миграции:", migratedData);

                    if (migratedData.version === 3) {
                        employees = migratedData.employees
                            .map(e => {
                                try {
                                    const emp = new Employee({
    ...e,
    carryOverHours: e.carryOverHours || {}  // Добавили переносимые часы
});
                                    if (!validateEmployee(emp)) {
                                        console.error("Некорректные данные работника", e);
                                        return null;
                                    }
                                    return emp;
                                } catch (error) {
                                    console.error("Ошибка создания работника:", e, error);
                                    return null;
                                }
                            })
                            .filter(emp => emp !== null);
                        
                        lunchSettings = migratedData.lunchSettings || {};
                        console.log(`Успешно загружено ${employees.length} работников`);
                    }
                } catch (e) {
                    console.error('Ошибка загрузки сохраненных данных:', e);
                    localStorage.removeItem('scheduleAppData');
                }
            }

            if (localStorage.getItem('theme') === 'dark') {
                document.body.classList.add('dark-theme');
            }

            generateCalendar();

            document.getElementById('lunchSettingsBtn').addEventListener('click', () => {
                document.querySelectorAll('#lunchTable input').forEach(input => {
                    const key = input.dataset.schedule;
                    input.value = lunchSettings[key] || 0;
                });
                document.getElementById('lunchModal').style.display = 'flex';
            });

            document.getElementById('fileInput').addEventListener('change', function(e) {
                const file = e.target.files[0];
                if (!file) return;

                const reader = new FileReader();
                reader.onload = (event) => {
                    try {
                        console.log("Загрузка файла...");
                        const data = JSON.parse(event.target.result);
                        console.log("Данные из файла:", data);

                        // Миграция данных из старых версий
                        const migratedData = migrateData(data);
                        console.log("Данные после миграции:", migratedData);

                        if (migratedData.version === 3) {
                            // Создание и валидация работников
                            employees = migratedData.employees
                                .map(e => {
                                    try {
                                        const emp = new Employee(e);
                                        if (!validateEmployee(emp)) {
                                            console.error("Некорректные данные работника", e);
                                            return null;
                                        }
                                        return emp;
                                    } catch (error) {
                                        console.error("Ошибка создания работника:", e, error);
                                        return null;
                                    }
                                })
                                .filter(emp => emp !== null);
                            
                            lunchSettings = migratedData.lunchSettings || {};
                            
                            saveData();
                            generateCalendar();
                            alert(`Успешно загружено ${employees.length} работников`);
                        } else {
                            alert('Неподдерживаемая версия файла!');
                        }
                    } catch (error) {
                        console.error("Ошибка загрузки:", error);
                        alert('Ошибка загрузки файла: ' + error.message);
                    } finally {
                        e.target.value = ''; // Сброс для возможности повторной загрузки
                    }
                };
                reader.onerror = () => {
                    alert('Ошибка чтения файла');
                    e.target.value = '';
                };
                reader.readAsText(file);
            });
        });

        window.onclick = function(event) {
            if (event.target.classList.contains('modal')) {
                event.target.style.display = 'none';
            }
        };
    </script>
</body>
</html>
