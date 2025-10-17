[TRABAJANDO.html](https://github.com/user-attachments/files/22963944/TRABAJANDO.html)
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Formulario Completo de Operativo Policial</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
            line-height: 1.6;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            border-radius: 15px;
            box-shadow: 0 15px 40px rgba(0,0,0,0.2);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            color: white;
            padding: 35px;
            text-align: center;
        }

        .header h1 {
            font-size: 2.3rem;
            margin-bottom: 12px;
            font-weight: 700;
        }

        .header p {
            font-size: 1.1rem;
            opacity: 0.9;
            font-weight: 300;
        }

        .form-container {
            padding: 35px;
        }

        .section {
            background: #f8f9fa;
            border-radius: 12px;
            padding: 28px;
            margin-bottom: 28px;
            border-left: 6px solid #2a5298;
            transition: transform 0.2s ease;
        }

        .section:hover {
            transform: translateY(-2px);
        }

        .section-title {
            font-size: 1.4rem;
            color: #2a5298;
            margin-bottom: 22px;
            border-bottom: 2px solid #dee2e6;
            padding-bottom: 12px;
            display: flex;
            align-items: center;
            gap: 10px;
            font-weight: 600;
        }

        /* Estilos para el mapa */
        .map-container {
            margin-top: 15px;
            border-radius: 8px;
            overflow: hidden;
            border: 2px solid #e9ecef;
            display: none;
        }

        #map {
            height: 400px;
            width: 100%;
        }

        .map-controls {
            display: flex;
            gap: 10px;
            margin-top: 10px;
            flex-wrap: wrap;
        }

        .map-search-container {
            flex: 1;
            min-width: 250px;
            position: relative;
        }

        .map-search-input {
            width: 100%;
            padding: 12px;
            border: 2px solid #e9ecef;
            border-radius: 8px;
            font-size: 1rem;
        }

        .map-search-results {
            position: absolute;
            top: 100%;
            left: 0;
            right: 0;
            background: white;
            border: 1px solid #e9ecef;
            border-radius: 0 0 8px 8px;
            max-height: 200px;
            overflow-y: auto;
            z-index: 1000;
            display: none;
        }

        .map-search-result {
            padding: 10px 12px;
            cursor: pointer;
            border-bottom: 1px solid #f1f3f5;
            transition: background-color 0.2s ease;
        }

        .map-search-result:hover {
            background-color: #f8f9fa;
        }

        .selected-location-info {
            background-color: #e7f3ff;
            border: 1px solid #b3d9ff;
            border-radius: 8px;
            padding: 12px;
            margin-top: 10px;
            display: none;
        }

        .selected-location-info h4 {
            margin-bottom: 8px;
            color: #004085;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .location-details {
            font-size: 0.9rem;
            color: #004085;
        }

        .subsection-title {
            font-size: 1.2rem;
            color: #495057;
            margin: 20px 0 15px;
            padding-bottom: 8px;
            border-bottom: 1px solid #dee2e6;
        }

        .form-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
        }

        .form-group {
            display: flex;
            flex-direction: column;
            position: relative;
        }

        .form-group-full {
            grid-column: 1 / -1;
        }

        label {
            font-weight: 600;
            margin-bottom: 8px;
            color: #495057;
            font-size: 0.95rem;
        }

        .required::after {
            content: " *";
            color: #dc3545;
        }

        input, select, textarea {
            padding: 12px;
            border: 2px solid #e9ecef;
            border-radius: 8px;
            font-size: 1rem;
            background: white;
            transition: all 0.3s ease;
            font-family: inherit;
        }

        input:focus, select:focus, textarea:focus {
            outline: none;
            border-color: #2a5298;
            box-shadow: 0 0 0 3px rgba(42,82,152,0.15);
        }

        .readonly {
            background: #f1f3f5;
            cursor: not-allowed;
            color: #495057;
        }

        .auto-filled {
            background-color: #e8f5e8 !important;
            border-color: #28a745 !important;
            color: #155724 !important;
        }

        .data-status {
            font-size: 0.85rem;
            margin-top: 5px;
            padding: 5px;
            border-radius: 5px;
            font-weight: 500;
        }

        .loading {
            background: #fff3cd;
            color: #856404;
            border: 1px solid #ffeaa7;
        }

        .success {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .error {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .field-error {
            color: #dc3545;
            font-size: 0.85rem;
            margin-top: 5px;
            display: none;
            font-weight: 500;
        }

        .validation-summary {
            background-color: #f8f9fa;
            border: 1px solid #dee2e6;
            border-radius: 8px;
            padding: 15px;
            margin-top: 15px;
            display: none;
        }

        .validation-summary.show {
            display: block;
        }

        .validation-summary.error {
            background-color: #f8d7da;
            border-color: #f5c6cb;
            color: #721c24;
        }

        .validation-summary.success {
            background-color: #d4edda;
            border-color: #c3e6cb;
            color: #155724;
        }

        .validation-summary.warning {
            background-color: #fff3cd;
            border-color: #ffeaa7;
            color: #856404;
        }

        .validation-summary-title {
            font-weight: 600;
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .validation-summary ul {
            margin-left: 20px;
        }

        .validation-summary li {
            margin-bottom: 5px;
        }

        .validation-icon {
            position: absolute;
            right: 12px;
            top: 38px;
            display: none;
            font-weight: bold;
        }

        .valid .validation-icon.check {
            display: block;
            color: #28a745;
        }

        .invalid .validation-icon.times {
            display: block;
            color: #dc3545;
        }

        .cedula-loading {
            position: absolute;
            right: 35px;
            top: 38px;
            display: none;
        }

        .cedula-loading.active {
            display: block;
            animation: spin 1s linear infinite;
        }

        small.hint {
            color: #6c757d;
            margin-top: 4px;
            font-size: 0.8rem;
            font-style: italic;
        }

        .btn-container {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            align-items: center;
            justify-content: center;
            margin-top: 25px;
            padding-top: 20px;
            border-top: 2px solid #e9ecef;
        }

        .btn {
            cursor: pointer;
            border: none;
            border-radius: 50px;
            padding: 14px 28px;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 8px;
            transition: all 0.3s ease;
            font-size: 1rem;
        }

        .btn-submit {
            background: linear-gradient(135deg, #28a745, #20c997);
            color: white;
        }

        .btn-submit:hover {
            background: linear-gradient(135deg, #218838, #1ea085);
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(40, 167, 69, 0.3);
        }

        .btn-secondary {
            background: linear-gradient(135deg, #6c757d, #495057);
            color: white;
        }

        .btn-secondary:hover {
            background: linear-gradient(135deg, #5a6268, #343a40);
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(108, 117, 125, 0.3);
        }

        .btn-map {
            background: linear-gradient(135deg, #17a2b8, #138496);
            color: white;
            padding: 10px 20px;
            font-size: 0.9rem;
        }

        .btn-map:hover {
            background: linear-gradient(135deg, #138496, #117a8b);
            transform: translateY(-2px);
        }

        .location-container {
            display: flex;
            gap: 10px;
            align-items: center;
        }

        .location-status {
            display: flex;
            align-items: center;
            gap: 5px;
            font-size: 0.85rem;
            margin-top: 5px;
            font-weight: 500;
        }

        .location-success {
            color: #28a745;
        }

        .location-error {
            color: #dc3545;
        }

        .location-warning {
            color: #ffc107;
        }

        .location-loading {
            color: #ffc107;
            animation: pulse 1.5s infinite;
        }

        .delito-search-container {
            position: relative;
        }

        .delito-search-results {
            position: absolute;
            top: 100%;
            left: 0;
            right: 0;
            background: white;
            border: 1px solid #e9ecef;
            border-radius: 0 0 8px 8px;
            max-height: 200px;
            overflow-y: auto;
            z-index: 1000;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
            display: none;
        }

        .delito-search-result {
            padding: 10px 12px;
            cursor: pointer;
            border-bottom: 1px solid #f1f3f5;
            transition: background-color 0.2s ease;
        }

        .delito-search-result:hover {
            background-color: #f8f9fa;
        }

        .delito-search-result:last-child {
            border-bottom: none;
        }

        .progress-container {
            margin-top: 15px;
            background-color: #e9ecef;
            border-radius: 10px;
            height: 8px;
            overflow: hidden;
        }

        .progress-bar {
            height: 100%;
            background: linear-gradient(135deg, #2a5298, #667eea);
            width: 0%;
            transition: width 0.5s ease;
        }

        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 20px;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.15);
            z-index: 10000;
            display: flex;
            align-items: center;
            gap: 10px;
            max-width: 400px;
            transform: translateX(150%);
            transition: transform 0.3s ease;
        }

        .notification.show {
            transform: translateX(0);
        }

        .notification.success {
            background-color: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .notification.error {
            background-color: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .notification.warning {
            background-color: #fff3cd;
            color: #856404;
            border: 1px solid #ffeaa7;
        }

        .notification.info {
            background-color: #d1ecf1;
            color: #0c5460;
            border: 1px solid #bee5eb;
        }

        .notification-close {
            background: none;
            border: none;
            font-size: 1.2rem;
            cursor: pointer;
            margin-left: auto;
            color: inherit;
        }

        .permission-help {
            background-color: #fff3cd;
            border: 1px solid #ffeaa7;
            border-radius: 8px;
            padding: 12px;
            margin-top: 10px;
            font-size: 0.85rem;
        }

        .permission-help h4 {
            margin-bottom: 8px;
            color: #856404;
        }

        .permission-help ul {
            padding-left: 20px;
        }

        .permission-help li {
            margin-bottom: 5px;
        }

        .detenido-container {
            border: 1px solid #dee2e6;
            border-radius: 8px;
            padding: 20px;
            margin-bottom: 15px;
            background: white;
            position: relative;
            transition: all 0.3s ease;
        }

        .detenido-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px solid #dee2e6;
        }

        .detenido-title {
            font-weight: 600;
            color: #495057;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .detenido-count {
            background-color: #2a5298;
            color: white;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.8rem;
        }

        .remove-detenido {
            background: #dc3545;
            color: white;
            border: none;
            border-radius: 50%;
            width: 30px;
            height: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-weight: bold;
        }

        .remove-detenido:hover {
            background: #c82333;
        }

        .variables-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 15px;
        }

        .variable-group {
            display: flex;
            flex-direction: column;
        }

        .variable-group label {
            font-size: 0.9rem;
            margin-bottom: 5px;
        }

        .variable-group input, .variable-group select {
            padding: 10px;
            font-size: 0.9rem;
        }

        .auto-detention-info {
            background-color: #e7f3ff;
            border: 1px solid #b3d9ff;
            border-radius: 8px;
            padding: 12px;
            margin-top: 10px;
            font-size: 0.85rem;
            color: #004085;
        }

        .auto-detention-info h4 {
            margin-bottom: 8px;
            color: #004085;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        /* Nuevos estilos para secciones de variables de productividad */
        .variable-section {
            display: none;
            margin-top: 20px;
            padding: 20px;
            background: #f8f9fa;
            border-radius: 8px;
            border-left: 4px solid #2a5298;
        }

        .variable-section.active {
            display: block;
            animation: fadeIn 0.3s ease;
        }

        .variable-input {
            border: 2px solid #e9ecef;
            transition: all 0.3s ease;
        }

        .variable-input.valid {
            border-color: #28a745;
            background-color: #f8fff8;
        }

        .variable-input.invalid {
            border-color: #dc3545;
            background-color: #fff8f8;
        }

        .variable-unit {
            color: #6c757d;
            font-size: 0.85rem;
            margin-left: 5px;
            font-style: italic;
        }

        .variable-input-container {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .variable-input-container input {
            flex: 1;
        }

        .validation-summary-item {
            display: flex;
            align-items: center;
            gap: 8px;
            margin-bottom: 5px;
            padding: 8px;
            border-radius: 5px;
            background-color: rgba(255,255,255,0.7);
        }

        .validation-summary-item.valid {
            color: #28a745;
            border-left: 4px solid #28a745;
        }

        .validation-summary-item.invalid {
            color: #dc3545;
            border-left: 4px solid #dc3545;
        }

        .auto-calculation {
            background-color: #e7f3ff;
            border: 1px solid #b3d9ff;
            border-radius: 8px;
            padding: 12px;
            margin-top: 10px;
            font-size: 0.85rem;
        }

        .auto-calculation h4 {
            margin-bottom: 8px;
            color: #004085;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        /* Estilos para indicadores adicionales */
        .indicadores-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }

        .indicador-group {
            display: flex;
            flex-direction: column;
        }

        .indicador-group label {
            font-size: 0.9rem;
            margin-bottom: 5px;
            font-weight: 600;
        }

        .indicador-group input, .indicador-group select {
            padding: 10px;
            border: 2px solid #e9ecef;
            border-radius: 6px;
            font-size: 0.9rem;
        }

        .indicador-group input:focus, .indicador-group select:focus {
            outline: none;
            border-color: #2a5298;
            box-shadow: 0 0 0 2px rgba(42,82,152,0.15);
        }

        /* Estilos para la sección de indicadores adicionales cuando se selecciona SI */
        .indicadores-adicionales {
            display: none;
            margin-top: 20px;
            padding: 20px;
            background: #f8f9fa;
            border-radius: 8px;
            border-left: 4px solid #2a5298;
        }

        .indicadores-adicionales.active {
            display: block;
            animation: fadeIn 0.3s ease;
        }

        .actions-container {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 20px;
        }

        .btn-add {
            background: linear-gradient(135deg, #17a2b8, #138496);
            color: white;
            padding: 10px 20px;
            font-size: 0.9rem;
        }

        .btn-add:hover {
            background: linear-gradient(135deg, #138496, #117a8b);
            transform: translateY(-2px);
        }

        .detenido-counter {
            font-weight: 600;
            color: #495057;
            background: #e9ecef;
            padding: 8px 15px;
            border-radius: 20px;
        }

        .small-hint {
            color: #6c757d;
            margin-top: 4px;
            font-size: 0.8rem;
            font-style: italic;
        }

        /* Estilos para el campo de archivo PowerPoint */
        .file-upload-container {
            border: 2px dashed #e9ecef;
            border-radius: 8px;
            padding: 20px;
            text-align: center;
            transition: all 0.3s ease;
            background: #f8f9fa;
        }

        .file-upload-container:hover {
            border-color: #2a5298;
            background: #f1f3f5;
        }

        .file-upload-container.dragover {
            border-color: #28a745;
            background: #e8f5e8;
        }

        .file-upload-icon {
            font-size: 2rem;
            margin-bottom: 10px;
            color: #6c757d;
        }

        .file-upload-text {
            margin-bottom: 15px;
        }

        .file-upload-btn {
            background: linear-gradient(135deg, #2a5298, #667eea);
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s ease;
        }

        .file-upload-btn:hover {
            background: linear-gradient(135deg, #1e3c72, #5a67d8);
            transform: translateY(-1px);
        }

        .file-info {
            margin-top: 10px;
            padding: 10px;
            background: #e7f3ff;
            border-radius: 6px;
            border: 1px solid #b3d9ff;
            display: none;
        }

        .file-info.show {
            display: block;
        }

        .file-name {
            font-weight: 600;
            color: #004085;
        }

        .file-size {
            color: #6c757d;
            font-size: 0.85rem;
        }

        .file-remove {
            background: none;
            border: none;
            color: #dc3545;
            cursor: pointer;
            margin-left: 10px;
            font-weight: bold;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @media (max-width: 768px) {
            .form-grid {
                grid-template-columns: 1fr;
            }
            
            .header h1 {
                font-size: 1.8rem;
            }
            
            .form-container {
                padding: 20px;
            }
            
            .section {
                padding: 20px;
            }
            
            .location-container {
                flex-direction: column;
                align-items: stretch;
            }
            
            .notification {
                left: 20px;
                right: 20px;
                max-width: none;
            }
            
            .variables-grid {
                grid-template-columns: 1fr;
            }
            
            .map-controls {
                flex-direction: column;
            }
            
            .map-search-container {
                min-width: 100%;
            }
            
            .variable-input-container {
                flex-direction: column;
                align-items: stretch;
            }
            
            .variable-unit {
                margin-left: 0;
                margin-top: 5px;
            }
            
            .indicadores-grid {
                grid-template-columns: 1fr;
            }
            
            .actions-container {
                flex-direction: column;
                gap: 15px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>📋 Formulario Completo de Operativo Policial</h1>
            <p>Sistema Integrado de Registro de Intervenciones y Detenidos</p>
        </div>

        <div class="form-container">
            <form id="operativeForm" autocomplete="off">
                <!-- Información General del Operativo -->
                <div class="section">
                    <h2 class="section-title">📅 Información General del Operativo</h2>
                    <div class="form-grid">
                        <div class="form-group">
                            <label for="fechaOperativo" class="required">Fecha del Operativo:</label>
                            <input type="date" id="fechaOperativo" name="fechaOperativo" required>
                            <div class="field-error" id="fechaOperativoError">La fecha es requerida</div>
                        </div>
                        <div class="form-group">
                            <label for="horaOperativo" class="required">Hora del Operativo:</label>
                            <input type="time" id="horaOperativo" name="horaOperativo" required>
                            <div class="field-error" id="horaOperativoError">La hora es requerida</div>
                        </div>
                        <div class="form-group">
                            <label for="direccionInterventora" class="required">Dirección Interventora:</label>
                            <select id="direccionInterventora" name="direccionInterventora" required>
                                <option value="">Seleccione una dirección...</option>
                                <option value="DNPJ">DNPJ</option>
                                <option value="DNA">DNA</option>
                                <option value="DINASED">DINASED</option>
                                <option value="DINIC">DINIC</option>
                                <option value="DINAF">DINAF</option>
                                <option value="DINITEC">DINITEC</option>
                                <option value="DNCTSV">DNCTSV</option>
                                <option value="DNPOLCO">DNPOLCO</option>
                                <option value="UNIDADES TRANSVERSALES">UNIDADES TRANSVERSALES</option>
                                <option value="DGI">DGI</option>
                                <option value="OTRO SERVICIO">OTRO SERVICIO</option>
                            </select>
                            <div class="field-error" id="direccionInterventoraError">Seleccione una dirección interventora</div>
                        </div>
                        <div class="form-group">
                            <label for="unidadInterventora" class="required">Unidad Interventora:</label>
                            <select id="unidadInterventora" name="unidadInterventora" required disabled>
                                <option value="">Primero seleccione una dirección...</option>
                            </select>
                            <div class="field-error" id="unidadInterventoraError">Seleccione una unidad interventora</div>
                            <div class="data-status" id="unidadesStatus"></div>
                        </div>
                        <div class="form-group">
                            <label for="codigoSubcircuito">Código de Subcircuito:</label>
                            <input type="text" id="codigoSubcircuito" name="codigoSubcircuito" 
                                   placeholder="Ej: 13D08C05S01">
                            <small class="hint">Ingrese el código del subcircuito. Los datos de Zona, Subzona, etc. se cargarán automáticamente.</small>
                            <div class="data-status" id="dataStatus"></div>
                        </div>
                    </div>
                    
                    <div class="form-grid">
                        <div class="form-group">
                            <label for="zona">Zona:</label>
                            <input type="text" id="zona" name="zona" class="readonly" readonly>
                        </div>
                        <div class="form-group">
                            <label for="subzona">Subzona:</label>
                            <input type="text" id="subzona" name="subzona" class="readonly" readonly>
                        </div>
                        <div class="form-group">
                            <label for="distrito">Distrito:</label>
                            <input type="text" id="distrito" name="distrito" class="readonly" readonly>
                        </div>
                        <div class="form-group">
                            <label for="circuito">Circuito:</label>
                            <input type="text" id="circuito" name="circuito" class="readonly" readonly>
                        </div>
                        <div class="form-group">
                            <label for="subcircuito">Subcircuito:</label>
                            <input type="text" id="subcircuito" name="subcircuito" class="readonly" readonly>
                        </div>
                    </div>
                    
                    <div class="form-grid">
                        <div class="form-group">
                            <label for="latitud" class="required">Latitud:</label>
                            <div class="location-container">
                                <input type="text" id="latitud" name="latitud" 
                                       placeholder="Ej: -0,229850" 
                                       pattern="^-?\d{1,2},\d+$"
                                       required>
                                <button type="button" id="btnObtenerUbicacion" class="btn btn-submit">📍 GPS</button>
                                <button type="button" id="btnMostrarMapa" class="btn btn-map">🗺️ Mapa</button>
                            </div>
                            <div class="field-error" id="latitudError">La latitud debe usar coma como separador decimal (Ej: -0,229850)</div>
                            <div class="location-status" id="locationStatus"></div>
                            <div class="permission-help" id="permissionHelp" style="display: none;">
                                <h4>Permiso de ubicación denegado</h4>
                                <p>Para obtener la ubicación automáticamente:</p>
                                <ul>
                                    <li>Haga clic en el icono de candado en la barra de direcciones</li>
                                    <li>Permita el acceso a la ubicación</li>
                                    <li>O intente nuevamente haciendo clic en "Obtener Ubicación"</li>
                                </ul>
                                <p>Alternativamente, puede ingresar manualmente las coordenadas usando coma como separador decimal.</p>
                            </div>
                        </div>
                        <div class="form-group">
                            <label for="longitud" class="required">Longitud:</label>
                            <input type="text" id="longitud" name="longitud" 
                                   placeholder="Ej: -78,524950" 
                                   pattern="^-?\d{1,3},\d+$"
                                   required>
                            <div class="field-error" id="longitudError">La longitud debe usar coma como separador decimal (Ej: -78,524950)</div>
                        </div>
                    </div>

                    <!-- Nuevo campo de Dirección -->
                    <div class="form-grid">
                        <div class="form-group form-group-full">
                            <label for="direccion">Dirección:</label>
                            <textarea id="direccion" name="direccion" rows="3" 
                                      class="readonly"
                                      readonly
                                      placeholder="La dirección se cargará automáticamente al seleccionar una ubicación en el mapa o usar GPS..."></textarea>
                            <small class="hint">Este campo se llena automáticamente con la dirección correspondiente a las coordenadas seleccionadas</small>
                        </div>
                    </div>

                    <!-- Contenedor del Mapa -->
                    <div class="map-container" id="mapContainer">
                        <div class="map-controls">
                            <div class="map-search-container">
                                <input type="text" id="mapSearch" class="map-search-input" 
                                       placeholder="Buscar dirección, lugar o punto de referencia...">
                                <div class="map-search-results" id="mapSearchResults"></div>
                            </div>
                            <button type="button" id="btnOcultarMapa" class="btn btn-secondary">❌ Ocultar Mapa</button>
                        </div>
                        <div id="map"></div>
                        <div class="selected-location-info" id="selectedLocationInfo">
                            <h4>📍 Ubicación Seleccionada</h4>
                            <div class="location-details" id="locationDetails">
                                Coordenadas: <span id="selectedCoords"></span><br>
                                Dirección: <span id="selectedAddress">Cargando...</span>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Presunto Delito Intervenido -->
                <div class="section">
                    <h2 class="section-title">⚖️ Presunto Delito Intervenido</h2>
                    <div class="form-grid">
                        <div class="form-group delito-search-container form-group-full">
                            <label for="presuntoDelito" class="required">Presunto Delito:</label>
                            <input type="text" id="presuntoDelito" name="presuntoDelito" 
                                   placeholder="Escriba para buscar delito..." required>
                            <div class="delito-search-results" id="delitoSearchResults"></div>
                            <div class="field-error" id="delitoError">Seleccione un presunto delito</div>
                            <small class="hint">Escriba para buscar entre los delitos disponibles</small>
                        </div>
                    </div>
                    
                    <div class="form-grid">
                        <div class="form-group form-group-full">
                            <label for="extractoCaso" class="required">Extracto del Caso:</label>
                            <textarea id="extractoCaso" name="extractoCaso" rows="4" 
                                      placeholder="Describa brevemente los hechos ocurridos, contexto y detalles relevantes del caso..." 
                                      required></textarea>
                            <div class="field-error" id="extractoCasoError">El extracto del caso es requerido</div>
                            <small class="hint">Resumen narrativo de los hechos, incluyendo contexto, participantes y detalles relevantes</small>
                        </div>
                    </div>
                    
                    <div class="form-grid">
                        <div class="form-group">
                            <label for="tipoEjecucion" class="required">Tipo de Ejecución de la Operación:</label>
                            <select id="tipoEjecucion" name="tipoEjecucion" required>
                                <option value="">Seleccione...</option>
                                <option value="ACTO ADMINISTRATIVO">ACTO ADMINISTRATIVO</option>
                                <option value="ACTO URGENTE">ACTO URGENTE</option>
                                <option value="BOLETA DE DETENCIÓN">BOLETA DE DETENCIÓN</option>
                                <option value="FLAGRANCIA">FLAGRANCIA</option>
                                <option value="INVESTIGACIÓN PREVIA">INVESTIGACIÓN PREVIA</option>                                
                            </select>
                            <div class="field-error" id="tipoEjecucionError">Seleccione un tipo de ejecución</div>
                        </div>
                        
                        <div class="form-group">
                            <label for="tipoCoordinacion" class="required">Tipo de Coordinación:</label>
                            <select id="tipoCoordinacion" name="tipoCoordinacion" required>
                                <option value="">Seleccione...</option>
                                <option value="ACTO ADMINISTRATIVO">ACTO ADMINISTRATIVO</option>
                                <option value="COORDINACIÓN INSTITUCIONAL">COORDINACIÓN INSTITUCIONAL</option>
                                <option value="COORDINACIÓN INTERNACIONAL">COORDINACIÓN INTERNACIONAL</option>                                
                            </select>
                            <div class="field-error" id="tipoCoordinacionError">Seleccione un tipo de coordinación</div>
                        </div>
                    </div>
                    
                    <div class="form-grid">
                        <div class="form-group">
                            <label for="caracterizacionAprehendido" class="required">Caracterización del Aprehendido/Detenido:</label>
                            <select id="caracterizacionAprehendido" name="caracterizacionAprehendido" required>
                                <option value="">Seleccione una caracterización...</option>
                                <option value="ACTO ADMINISTRATIVO">ACTO ADMINISTRATIVO</option>
                                <option value="ADMINISTRACIÓN DE JUSTICIA">ADMINISTRACIÓN DE JUSTICIA</option>
                                <option value="ASAMBLEÍSTAS">ASAMBLEÍSTAS</option>
                                <option value="DELINCUENCIA COMÚN">DELINCUENCIA COMÚN</option>
                                <option value="DIFUSIÓN ROJA">DIFUSIÓN ROJA</option>
                                <option value="FISCALES">FISCALES</option>
                                <option value="FUNCIONARIO DE GOBIERNO">FUNCIONARIO DE GOBIERNO</option>
                                <option value="JUECES">JUECES</option>
                                <option value="MÁS BUSCADO">MÁS BUSCADO</option>
                                <option value="MILITARES">MILITARES</option>
                                <option value="MINISTROS">MINISTROS</option>
                                <option value="OBJETIVO DE ALTO VALOR (OAV)">OBJETIVO DE ALTO VALOR (OAV)</option>
                                <option value="OBJETIVO DE INTERMEDIO VALOR (OIV)">OBJETIVO DE INTERMEDIO VALOR (OIV)</option>
                                <option value="POLICÍAS">POLICÍAS</option>
                                <option value="SERVIDORES PÚBLICOS">SERVIDORES PÚBLICOS</option>
                            </select>
                            <div class="field-error" id="caracterizacionAprehendidoError">Seleccione una caracterización</div>
                            <small class="hint">Seleccione el tipo de caracterización del aprehendido/detenido</small>
                        </div>
                    </div>
                </div>

                <!-- Variables de Productividad -->
                <div class="section" id="seccionVariablesProductividad">
                    <h2 class="section-title">📈 Variables de Productividad</h2>
                    
                    <div class="form-grid">
                        <div class="form-group">
                            <label for="variablesProductividad" class="required">Variables de Productividad:</label>
                            <select id="variablesProductividad" name="variablesProductividad" required>
                                <option value="">Seleccione una variable...</option>
                                <option value="Aeronaves (Und)">Aeronaves</option>
                                <option value="Allanamientos">Allanamientos</option>
                                <option value="Aprehendidos/Detenidos">Aprehendidos/Detenidos</option>
                                <option value="Armas de Fuego">Armas de Fuego</option>
                                <option value="Auditorias de Seguridad">Auditorias de Seguridad</option>
                                <option value="Autopartes y Autopartes de Vehiculos">Autopartes y Autopartes de Vehiculos</option>
                                <option value="Bienes Inmuebles Incautados">Bienes Inmuebles Incautados</option>
                                <option value="Cantidad Personas Desaparecidas Localizadas">Cantidad Personas Desaparecidas Localizadas</option>
                                <option value="Cap_Detonantes (Und)">Cap_Detonantes</option>
                                <option value="Celulares (Und)">Celulares</option>
                                <option value="Certificado de Armas Entregado">Certificado de Armas Entregado</option>
                                <option value="Cloruro de Calcio (Kg)">Cloruro de Calcio</option>
                                <option value="Coincidencias Potenciales de Vainas">Coincidencias Potenciales de Vainas</option>
                                <option value="Coordinacion con Upc">Coordinacion con Upc</option>
                                <option value="Combustibles">Combustibles</option>
                                <option value="Coordinaciones Realizadas">Coordinaciones Realizadas</option>
                                <option value="Deposito de Dinero Real">Deposito de Dinero Real</option>
                                <option value="Destiladores">Destiladores</option>
                                <option value="Destruccion de Dinero Falso">Destruccion de Dinero Falso</option>
                                <option value="Dinero en Cuentas Bancarias ($)">Dinero en Cuentas Bancarias</option>
                                <option value="Disposicion Final de Armas de Fuego">Disposicion Final de Armas de Fuego</option>
                                <option value="Disposicion Final de Automotores">Disposicion Final de Automotores</option>
                                <option value="Disposicion Final de Bienes Muebles E Inmuebles">Disposicion Final de Bienes Muebles e Inmuebles</option>
                                <option value="Disposicion Final de Municiones">Disposicion Final de Municiones</option>
                                <option value="Dolares Americanos ($)">Dolares Americanos</option>
                                <option value="Dolares Falsos ($)">Dolares Falsos</option>
                                <option value="Drogas">Drogas</option>
                                <option value="Embarcaciones (Und)">Embarcaciones</option>
                                <option value="Fauna Silvestre (Und)">Fauna Silvestre</option>
                                <option value="Fulminantes (Und)">Fulminantes</option>
                                <option value="Glp (15 - 45 Kg) (Und)">Glp (15 - 45 Kg)</option>
                                <option value="Granadas (Und)">Granadas</option>
                                <option value="Indique El Porcentaje de Cumplimiento de La Investigacion Previa (%)">Indique El Porcentaje de Cumplimiento de La Investigacion Previa</option>
                                <option value="Informacion Relevante (Sms-Audios)">Informacion Relevante (Sms-Audios)</option>
                                <option value="Informes de Analisis">Informes de Analisis</option>
                                <option value="Investigaciones Previas Aperturadas">Investigaciones Previas Aperturadas</option>
                                <option value="Laboratorio_Procesamiento_Droga">Laboratorio_Procesamiento_Droga</option>
                                <option value="Madera (M3)">Madera</option>
                                <option value="Maquinaria Pesada (Und)">Maquinaria Pesada</option>
                                <option value="Material Aurifero (Kg)">Material Aurifero</option>
                                <option value="Mecha Lenta Cordon Detonante (Metros)">Mecha Lenta Cordon Detonante</option>
                                <option value="Minas Detonantes (Und)">Minas Detonantes</option>
                                <option value="Motores F. Borda (Und)">Motores F. Borda</option>
                                <option value="Municiones (Und)">Municiones</option>
                                <option value="Niños, Niñas y Adolescentes Rescatados">Niños, Niñas y Adolescentes Rescatados</option>
                                <option value="Nitrato de Amonio (Kg)">Nitrato de Amonio</option>
                                <option value="Numero de La Investigacion Previa Aperturada">Numero de La Investigacion Previa Aperturada</option>
                                <option value="Operativos de Traslado">Operativos de Traslado</option>
                                <option value="Ordenes Judiciales de Analisis Telefonico Cumplidas">Ordenes Judiciales de Analisis Telefonico Cumplidas</option>
                                <option value="Patrimonio Cultural (Und)">Patrimonio Cultural</option>
                                <option value="Pentolita (Kg)">Pentolita</option>
                                <option value="Perdigones (Und)">Perdigones</option>
                                <option value="Perforaciones">Perforaciones</option>
                                <option value="Pericias Accidentologia Vial">Pericias Accidentologia Vial</option>
                                <option value="Pericias Criminalistica">Pericias Criminalistica</option>
                                <option value="Pericias Medicina Legal">Pericias Medicina Legal</option>
                                <option value="Personas Protegidas">Personas Protegidas</option>
                                <option value="Pirotecnia (Und)">Pirotecnia</option>
                                <option value="Piscinas Artesanales de Cldh">Piscinas Artesanales de Cldh</option>
                                <option value="Polvora (Kg)">Polvora</option>
                                <option value="Productos Agricolas (Bultos)">Productos Agricolas</option>
                                <option value="Quimicos Liquidos (Lt)">Quimicos Liquidos</option>
                                <option value="Quimicos Solidos (Kg)">Quimicos Solidos</option>
                                <option value="Registro de Medidas Cautelares">Registro de Medidas Cautelares</option>
                                <option value="Reportes Telefonicos Entregados">Reportes Telefonicos Entregados</option>
                                <option value="Semovientes (Und)">Semovientes</option>
                                <option value="Subzonas Intervenidas">Subzonas Intervenidas</option>
                                <option value="Tacos de Dinamita (Und)">Tacos de Dinamita</option>
                                <option value="Traslado de Indicios Bajo Pedido de Autoridad Competente">Traslado de Indicios Bajo Pedido de Autoridad Competente</option>
                                <option value="Valor de los Bienes Inmuebles Incautados">Valor de los Bienes Inmuebles Incautados</option>
                                <option value="Vehículos">Vehículos</option>
                                <option value="Victimas Liberadas">Victimas Liberadas</option>
                                <option value="Víctimas Rescatadas">Víctimas Rescatadas</option>
                                <option value="Varios">Varios</option>
                            </select>
                            <div class="field-error" id="variablesProductividadError">Seleccione una variable de productividad</div>
                            <small class="hint">Seleccione el tipo de variable de productividad del operativo</small>
                        </div>
                    </div>

                    <!-- Secciones dinámicas para cada variable -->
                    <div id="variableSectionsContainer">
                        <!-- Las secciones específicas se generarán dinámicamente aquí -->
                    </div>

                    <!-- Resumen de validaciones -->
                    <div class="validation-summary" id="validationSummary">
                        <h3 class="validation-summary-title">📋 Resumen de Validaciones</h3>
                        <div id="validationSummaryContent"></div>
                    </div>
                </div>

                <!-- Registro de Detenidos -->
                <div class="section" id="seccionDetenidos" style="display: none;">
                    <h2 class="section-title">👤 Registro de Detenidos</h2>
                    
                    <div id="detenidosContainer">
                        <!-- Los contenedores de detenidos se generarán dinámicamente aquí -->
                    </div>

                    <div class="actions-container">
                        <!-- EL BOTÓN AGREGAR DETENIDO HA SIDO ELIMINADO -->
                        <div class="detenido-counter">
                            <span id="totalDetenidos">0</span> detenido(s) registrado(s)
                        </div>
                    </div>
                </div>

                <!-- Indicadores Adicionales -->
                <div class="section" id="seccionIndicadoresAdicionales">
                    <h2 class="section-title">📊 Indicadores Adicionales</h2>
                    
                    <div class="form-grid">
                        <div class="form-group">
                            <label for="nivelIndicadores">Desea ingresar más indicadores?:</label>
                            <select id="nivelIndicadores" name="nivelIndicadores">
                                <option value="">Seleccione nivel...</option>
                                <option value="SI">SI</option>
                                <option value="NO">NO</option>
                            </select>
                            <small class="hint">Evaluación del nivel de satisfacción ciudadana con el operativo</small>
                        </div> 
                    </div>

                    <!-- Sección de indicadores adicionales (se muestra cuando se selecciona SI) -->
                    <div class="indicadores-adicionales" id="indicadoresAdicionales">
                    </div>
                </div>

                <!-- Información del Responsable -->
                <div class="section" id="seccionResponsable">
                    <h2 class="section-title">👮 Responsable del Reporte</h2>
                    <div class="form-grid">
                        <div class="form-group">
                            <label for="numeroCedula" class="required">Número de Cédula:</label>
                            <input type="text" id="numeroCedula" name="numeroCedula" 
                                   placeholder="Ej: 0401400783" 
                                   pattern="[0-9]{10}" 
                                   maxlength="10" 
                                   required>
                            <span class="cedula-loading">🔄</span>
                            <span class="validation-icon check">✓</span>
                            <span class="validation-icon times">✗</span>
                            <div class="field-error" id="cedulaError">La cédula debe tener 10 dígitos numéricos</div>
                            <small class="hint">Ingrese su número de cédula (10 dígitos). Los datos se cargarán automáticamente.</small>
                            <div class="data-status" id="cedulaStatus"></div>
                        </div>
                        <div class="form-group">
                            <label for="grado" class="required">Grado:</label>
                            <input type="text" id="grado" name="grado" 
                                   class="readonly" 
                                   readonly 
                                   placeholder="Se cargará automáticamente...">
                            <span class="validation-icon check">✓</span>
                            <span class="validation-icon times">✗</span>
                            <div class="field-error" id="gradoError">El grado se cargará automáticamente al ingresar la cédula</div>
                        </div>
                        <div class="form-group">
                            <label for="apellidosNombres" class="required">Apellidos y Nombres:</label>
                            <input type="text" id="apellidosNombres" name="apellidosNombres" 
                                   class="readonly"
                                   readonly
                                   placeholder="Se cargará automáticamente...">
                            <span class="validation-icon check">✓</span>
                            <span class="validation-icon times">✗</span>
                            <div class="field-error" id="nombresError">Los nombres se cargarán automáticamente al ingresar la cédula</div>
                        </div>
                        <div class="form-group">
                            <label for="celular" class="required">Celular:</label>
                            <input type="tel" id="celular" name="celular" 
                                   class="readonly"
                                   readonly
                                   placeholder="Se cargará automáticamente...">
                            <span class="validation-icon check">✓</span>
                            <span class="validation-icon times">✗</span>
                            <div class="field-error" id="celularError">El celular se cargará automáticamente al ingresar la cédula</div>
                            <small class="hint">Este campo se llena automáticamente con los datos del funcionario</small>
                        </div>
                        <div class="form-group">
                            <label for="correoElectronico" class="required">Correo Electronico:</label>
                            <input type="email" id="correoElectronico" name="correoElectronico" 
                                   class="readonly"
                                   readonly
                                   placeholder="Se cargará automáticamente...">
                            <span class="validation-icon check">✓</span>
                            <span class="validation-icon times">✗</span>
                            <div class="field-error" id="correoError">El correo se cargará automáticamente al ingresar la cédula</div>
                            <small class="hint">Este campo se llena automáticamente con los datos del funcionario</small>
                        </div>
                        
                        <!-- NUEVO CAMPO: Presentación en PowerPoint - AHORA OBLIGATORIO -->
                        <div class="form-group form-group-full">
                            <label for="presentacionPowerPoint" class="required">Agregue la presentación del caso en formato Power point:</label>
                            <div class="file-upload-container" id="powerpointUploadContainer">
                                <div class="file-upload-icon">📊</div>
                                <div class="file-upload-text">
                                    <p>Haga clic para seleccionar o arrastre aquí su archivo PowerPoint</p>
                                    <p><small>Formatos aceptados: .ppt, .pptx (Tamaño máximo: 50MB)</small></p>
                                </div>
                                <input type="file" 
                                       id="presentacionPowerPoint" 
                                       name="presentacionPowerPoint" 
                                       accept=".ppt,.pptx,application/vnd.ms-powerpoint,application/vnd.openxmlformats-officedocument.presentationml.presentation"
                                       required
                                       style="display: none;">
                                <button type="button" class="file-upload-btn" id="btnSeleccionarPowerPoint">Seleccionar Archivo</button>
                            </div>
                            <div class="file-info" id="powerpointFileInfo">
                                <span class="file-name" id="powerpointFileName">No se ha seleccionado ningún archivo</span>
                                <span class="file-size" id="powerpointFileSize"></span>
                                <button type="button" class="file-remove" id="btnRemoverPowerPoint">✕</button>
                            </div>
                            <div class="field-error" id="powerpointError">Debe adjuntar la presentación del caso en formato PowerPoint</div>
                            <small class="hint">Suba la presentación del caso en formato PowerPoint (.ppt o .pptx)</small>
                        </div>
                    </div>
                </div>

                <!-- Barra de progreso -->
                <div class="progress-container">
                    <div class="progress-bar" id="progressBar"></div>
                </div>

                <div class="btn-container">
                    <button type="submit" class="btn btn-submit">
                        📤 Enviar Respuestas
                    </button>
                    <!-- EL BOTÓN LIMPIAR FORMULARIO HA SIDO ELIMINADO -->
                </div>
            </form>
        </div>
    </div>

    <!-- Área de notificaciones -->
    <div id="notificationArea"></div>

    <!-- Agregar Leaflet JS -->
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script>
        // ========== CONFIGURACIÓN ==========
        const APPS_SCRIPT_URL_CEDULA = 'https://script.google.com/macros/s/AKfycby41-QTz-DEM9RYd8tw1np-ucs3minyBIRTi4yug13ir5sAvhp6_nIZBJRN4DdwFz8kHg/exec';
        const APPS_SCRIPT_URL_SUBCIRCUITO = 'https://script.google.com/macros/s/AKfycbxfR5YjNgpn9m6VDvoCFUoX3YnPAjbYQbiJoYVQvByWrUDVFE-mv_x7-eFa7DyH6KmBRw/exec';

        // ========== LISTA DE DELITOS ==========
        const listaDelitos = [
            "ABANDONO DE PERSONA", "ABIGEATO", "ABORTO CON MUERTE", "ABORTO CONSENTIDO", "ABORTO NO CONSENTIDO",
            "ABUSO DE ARMA DE FUEGO", "ABUSO DE CONFIANZA", "ABUSO DE EMBLEMAS", "ABUSO DE FACULTADES", "ABUSO SEXUAL",
            "ACOSO SEXUAL", "ACTIVIDAD ILICITA OF RECURSOS MINEROS", "ACTOS DE ODIO", "ACTOS HOSTILES CONTRA EL ESTADO",
            "ACUSACION O DENUNCIA MALICIOSA", "ADOPCION ILEGAL", "AGIOTAJE", "AGRESION", "ALTERACION DE EVIDENCIAS",
            "APOLOGIA", "APROPIACION FRAUDULENTA", "ARMAS DE FUEGO NO AUTORIZADAS", "ASESINATO", "ASOCIACION ILICITA",
            "ATAQUE A BIENES PROTEGIDOS", "ATAQUE O RESISTENCIA", "ATENTADO A LA INTEGRIDAD SEXUAL", "AUTORIZACION INDEBIDA",
            "CALUMNIA", "COHECHO", "COMERCIALIZACION DE PORNOGRAFIA", "COMERCIALIZACION ILICITA", "CONCUSION",
            "CONDUCCION BAJO EFECTO OF SUSTANCIAS", "CONDUCCION EN ESTADO OF EMBRIAGUEZ", "CONTACTO CON FINALIDAD SEXUAL",
            "CONTAMINACION OF SUSTANCIAS", "CONTABANDO", "CONTRAVENCION", "CORRUPCION OF MENORES", "DAÑO A BIEN AJENO",
            "DAÑO PERMANENTE A LA SALUD", "DAÑOS MATERIALES", "DEFRAUDACION ADUANERA", "DEFRAUDACION TRIBUTARIA",
            "DELINCUENCIA ORGANIZADA", "DELITOS CONTRA EL AGUA", "DELITOS CONTRA LA FLORA Y FAUNA", "DESAPARICION FORZADA",
            "DESERTACION", "DESTRUCCION DE BIENES", "DIFUSION DE INFORMACION", "DISCRIMINACION", "EJECUCION EXTRAJUDICIAL",
            "EJERCICIO ILEGAL OF LA PROFESION", "ENRIQUECIMIENTO ILICITO", "ESCLAVITUD", "ESPIONAJE", "ESTAFA", "ESTUPRO",
            "EVASION", "EXPLOTACION SEXUAL", "EXTERMINIO", "EXTORSION", "FALSA INCRIMINACION", "FALSEDAD DOCUMENTAL",
            "FALSIFICACION DE FIRMAS", "FALSIFICACION OF MONEDA", "FEMICIDIO", "FINANCIACION DEL TERRORISMO", "FRAUDE",
            "GENOCIDIO", "HOMICIDIO", "HOMICIDIO CULPOSO", "HURTO", "HURTO A DOMICILIO", "HURTO A ENTIDADES FINANCIERAS",
            "INCENDIO", "INCENDIOS FORESTALES", "INCITACION A DISCORDIA", "INFRACCIONES", "INSUBORDINACION", "INTIMIDACION",
            "INVASION DE AREAS", "LAVADO DE ACTIVOS", "LESIONES", "MALTRATO A ANIMALES", "MANIPULACION GENETICA",
            "MUERTE CULPOSA", "NEGATIVA A PRESTAR AUXILIO", "OBSTACULIZACION OF PROCESO", "OCULTAMIENTO DE INFORMACION",
            "OMISION OF DENUNCIA", "PECULADO", "PERJURIO", "PORNOGRAFIA CON MENORES", "PREVARICATO", "PRIVACION ILEGAL DE LIBERTAD",
            "PRODUCCION ILICITA", "RECEPTACION", "RECLUTAMIENTO DE MENORES", "REVELACION DE SECRETO", "ROBO", "ROBO A DOMICILIO",
            "SABOTAJE", "SECUESTRO", "SECUESTRO EXTORSIVO", "SICARIATO", "SUMINISTRO OF SUSTANCIAS", "SUPLANTACION DE IDENTIDAD",
            "TENENCIA Y PORTE DE ARMAS", "TENTATIVA", "TERRORISMO", "TOMA OF REHENES", "TORTURA", "TOMA OF REHENES",
            "TRAFICO DE INFLUENCIAS", "TRAFICO DE ORGANOS", "TRAFICO ILICITO DE ARMAS", "TRAFICO ILICITO DE MIGRANTES",
            "TRAFICO ILICITO OF SUSTANCIAS", "TRATA DE PERSONAS", "USURA", "USURPACION", "VIOLACION", "VIOLACION A LA INTIMIDAD",
            "VIOLENCIA CONTRA LA MUJER", "VIOLENCIA FISICA", "VIOLENCIA PSICOLOGICA", "VIOLENCIA SEXUAL"
        ];

        // ========== DATOS DE UNIDADES ==========
        const datosUnidades = {
            "DNPJ": ["BAC", "POLICIA JUDICIAL", "UICA", "UIDH", "UNIC", "UNIDAE", "UNIDCAN", "UNIF"],
            "DNA": ["COORDINADOS DNA", "JEFATURA", "GEMA", "UCTCI", "UIACE", "UIAN", "UIPA", "UNSQ"],
            "DINASED": ["UIDP", "UMV", "UNASE"],
            "DINIC": ["UCAP", "UDAR", "UNDECOF"],
            "DINAF": ["DEVIF", "DINAPEN", "UNAT", "UNCIS"],
            "DINITEC": ["CRIMINALISTICA", "UNIVIAL"],
            "DNCTSV": ["DNCTSV"],
            "DNPOLCO": ["DNPOLCO"],
            "UNIDADES TRANSVERSALES": ["AMERIPOL", "CIBERPOL", "INTERPOL", "UDT", "UIAD", "ULCO", "UNAI", "UNDP", "UNIT", "UNRED"],
            "DGI": ["DGI"],
            "OTRO SERVICIO": ["SEGURIDAD PENITENCIARIA", "ADUANA", "FUERZAS ARMADAS", "GUARDIA COSTERA EE.UU", "AGENTES MUNICIPALES"]
        };

        // ========== LISTA DE NACIONALIDADES ==========
        const nacionalidades = [
            "AFRICANA", "ALBANESA", "ALEMANA", "ANGOLENA", "ARGELINA", "ARGENTINA", "ARMENIA", "ASIATICA", "AUSTRALIANA", "AUTRALIANA", "AZERBAIJAN", 
            "BANGLADESI", "BELGA", "BELICENO",        "BENINES", "BIELORUSO", "BISAUGINEANA", "BOLIVIANA", "BRASILENA", "BRITANICA", "BULGARA", "BUTANESA", 
            "CABOVERDIANA", "CAMBOYANA", "CAMERUNES", "CANADIENSE", "CEILANA", "CEILANDESA", "CHADIANA", "CHECA", "CHILENA", "CHINA", "CHIPRIOTA", "COLOMBIANA", 
            "CONGOLESA", "COREANA", "COSTARRICENSE", "CROATA", "CUBANA", "DANESA", "DOMINICA", "DOMINICANA", "ECUATOGUINEANO", "ECUATORIANA", "EGIPCIO", 
            "ESPANOLA", "ESTADOUNIDENSE", "ESTONIA", "ESTONIANA", "ETIOPE", "FILIPINA", "FRANCESA", "GAMBIA", "GEORGIANA", "GHANESA", "GRIEGA", "GUANANO", 
            "GUATEMALTECA", "GUINEANA", "HAITIANA", "HINDU", "HOLANDESA", "HONDURENA", "HONGKONES", "HUNGARA", "INDONESA", "INDU", "INGLESA", "IRANI", 
            "IRLANDESA", "ISRAELITA", "ITALIANA", "JAPONESA", "JORDANA", "KIRGUINESA", "LETONIANA", "LIBANESA", "LIBIA", "LITUANA", "MACEDONIA", "MALASIA", 
            "MALAWI", "MALI", "MALTA", "MARFILENA", "MARROQUI", "MEXICANA", "MOLDAVA", "MONACO", "NEOZELANDESA", "NICARAGUENSE", "NIGERIANA", "NORTEAMERICANA", 
            "NORUEGA", "NUEVAGUINEANA", "PAKISTANI", "PALESTINA", "PANAMENA", "PAPUANA", "PARAGUAYA", "PERUANA", "POLACA", "PORTUGUESA", "PUERTORRIQUENA", 
            "REINO UNIDO", "RUMANA", "RUSA", "SAHARAUI", "SALOMONENSE", "SALVADORENA", "SAUDI", "SE DESCONOCE", "SENEGALES", "SERBIA", "SERBIO", "SIERRALEONESA", 
            "SINGAPURENSE", "SIRIA", "SOMALI", "SUDAFRICANA", "SUECA", "SUIZA", "SURCOREANA", "TAILANDESA", "TAIWANESA", "TAYIKA", "TOGOLES", "TURCA", "UCRANIANA", 
            "UGANDA", "UGANDESA", "URUGUAYA", "UZBECO",   "VANUATUENSE", "VATICANA", "VENEZOLANA", "VIETNAMITA", "YEMENI"
        ];

        // ========== LISTA DE GRUPOS DELICTIVOS ==========
        const gruposDelictivos = [
            "AGUILAS", "CHONE KILLERS", "CHONEROS", "FATALES",
            "GANGSTERS", "LAGARTOS", "LATIN KINGS", "LOBOS",
            "M18 TIBURONES", "R7", "TIGUERONES",
            "NINGUNO"
        ];

        // ========== LISTA DE RESOLUCIONES DE FLAGRANCIA ==========
        const resolucionesFlagrancia = [
            "AISLAMIENTO PREVENTIVO",
            "ARRESTO DOMICILIARIO", 
            "CONCILIACION",
            "EXPULSION DEL PAIS",
            "LIBRE CON INSTRUCCION FISCAL",
            "LIBRE CON INVESTIGACION PREVIA", 
            "LIBRE NO DELITO",
            "MEDIDA SUSTITUTIVA",
            "PRISION PREVENTIVA",
            "PROCEDIMIENTO ABREVIADO",
            "SE DESCONOCE"
        ];

        // ========== DEFINICIÓN DE VARIABLES DE PRODUCTIVIDAD ==========
        const variablesProductividad = {
            "Aeronaves (Und)": {
                tipo: "numero",
                unidad: "Unidades",
                requerido: true,
                placeholder: "0"
            },
            "Allanamientos": {
                tipo: "numero",
                unidad: "Allanamientos",
                requerido: true,
                placeholder: "0"
            },
            "Aprehendidos/Detenidos": {
                tipo: "detalle_aprehendidos",
                unidad: "Personas",
                requerido: true,
                placeholder: "0",
                campos: {
                    "cantidad_aprehendidos": { label: "CANTIDAD APREHENDIDOS", tipo: "numero", requerido: true, placeholder: "0" },
                    "cantidad_detenidos": { label: "CANTIDAD DETENIDOS", tipo: "numero", requerido: true, placeholder: "0" },
                    "cantidad_hombres": { label: "CANTIDAD HOMBRES", tipo: "numero", requerido: true, placeholder: "0" },
                    "cantidad_mujeres": { label: "CANTIDAD MUJERES", tipo: "numero", requerido: true, placeholder: "0" }
                },
                validaciones: [
                    {
                        nombre: "Suma de géneros vs aprehendidos/detenidos",
                        descripcion: "La suma de CANTIDAD MUJERES + CANTIDAD HOMBRES debe ser igual a la suma de CANTIDAD APREHENDIDOS + CANTIDAD DETENIDOS",
                        validar: function(campos) {
                            const sumaGeneros = parseFloat(campos.cantidad_hombres || 0) + parseFloat(campos.cantidad_mujeres || 0);
                            const sumaAprehendidosDetenidos = parseFloat(campos.cantidad_aprehendidos || 0) + parseFloat(campos.cantidad_detenidos || 0);
                            return sumaGeneros === sumaAprehendidosDetenidos;
                        }
                    }
                ]
            },
            "Armas de Fuego": {
                tipo: "detalle_armas",
                unidad: "Armas",
                requerido: true,
                placeholder: "0",
                campos: {
                    "total_armas": { label: "TOTAL ARMAS", tipo: "numero", requerido: false, placeholder: "0" },
                    "ametralladora": { label: "AMETRALLADORA", tipo: "numero", requerido: false, placeholder: "0" },
                    "bazuca": { label: "BAZUCA", tipo: "numero", requerido: false, placeholder: "0" },
                    "canon": { label: "CAÑON", tipo: "numero", requerido: false, placeholder: "0" },
                    "carabina": { label: "CARABINA", tipo: "numero", requerido: false, placeholder: "0" },
                    "cartuchera": { label: "CARTUCHERA", tipo: "numero", requerido: false, placeholder: "0" },
                    "escopeta": { label: "ESCOPETA", tipo: "numero", requerido: false, placeholder: "0" },
                    "fusil": { label: "FUSIL", tipo: "numero", requerido: false, placeholder: "0" },
                    "lanza_granada": { label: "LANZA GRANADA", tipo: "numero", requerido: false, placeholder: "0" },
                    "mortero": { label: "MORTERO", tipo: "numero", requerido: false, placeholder: "0" },
                    "pistola": { label: "PISTOLA", tipo: "numero", requerido: false, placeholder: "0" },
                    "pistolon": { label: "PISTOLON", tipo: "numero", requerido: false, placeholder: "0" },
                    "repetidora": { label: "REPETIDORA", tipo: "numero", requerido: false, placeholder: "0" },
                    "revolver": { label: "REVOLVER", tipo: "numero", requerido: false, placeholder: "0" },
                    "rifle": { label: "RIFLE", tipo: "numero", requerido: false, placeholder: "0" },
                    "subametralladora": { label: "SUBAMETRALLADORA", tipo: "numero", requerido: false, placeholder: "0" },
                    "otras_armas_fuego": { label: "OTRAS ARMAS DE FUEGO", tipo: "numero", requerido: false, placeholder: "0" }
                },
                validaciones: [
                    {
                        nombre: "Suma de tipos de armas vs total",
                        descripcion: "La suma de todos los tipos de armas debe ser igual al TOTAL ARMAS",
                        validar: function(campos) {
                            const sumaTiposArmas = 
                                parseFloat(campos.ametralladora || 0) +
                                parseFloat(campos.bazuca || 0) +
                                parseFloat(campos.canon || 0) +
                                parseFloat(campos.carabina || 0) +
                                parseFloat(campos.cartuchera || 0) +
                                parseFloat(campos.escopeta || 0) +
                                parseFloat(campos.fusil || 0) +
                                parseFloat(campos.lanza_granada || 0) +
                                parseFloat(campos.mortero || 0) +
                                parseFloat(campos.pistola || 0) +
                                parseFloat(campos.pistolon || 0) +
                                parseFloat(campos.repetidora || 0) +
                                parseFloat(campos.revolver || 0) +
                                parseFloat(campos.rifle || 0) +
                                parseFloat(campos.subametralladora || 0) +
                                parseFloat(campos.otras_armas_fuego || 0);
                            
                            const totalArmas = parseFloat(campos.total_armas || 0);
                            return sumaTiposArmas === totalArmas;
                        }
                    }
                ]
            },
            "Auditorias de Seguridad": {
                tipo: "numero",
                unidad: "Auditorías",
                requerido: true,
                placeholder: "0"
            },
            "Autopartes y Autopartes de Vehiculos": {
                tipo: "numero",
                unidad: "Piezas",
                requerido: true,
                placeholder: "0"
            },
            "Bienes Inmuebles Incautados": {
                tipo: "numero",
                unidad: "Bienes",
                requerido: true,
                placeholder: "0"
            },
            "Cantidad Personas Desaparecidas Localizadas": {
                tipo: "numero",
                unidad: "Personas",
                requerido: true,
                placeholder: "0"
            },
            "Cap_Detonantes (Und)": {
                tipo: "numero",
                unidad: "Unidades",
                requerido: true,
                placeholder: "0"
            },
            "Celulares (Und)": {
                tipo: "numero",
                unidad: "Unidades",
                requerido: true,
                placeholder: "0"
            },
            "Certificado de Armas Entregado": {
                tipo: "numero",
                unidad: "Certificados",
                requerido: true,
                placeholder: "0"
            },
            "Cloruro de Calcio (Kg)": {
                tipo: "numero",
                unidad: "Kilogramos",
                requerido: true,
                placeholder: "0.00",
                step: "0.01"
            },
            "Coincidencias Potenciales de Vainas": {
                tipo: "numero",
                unidad: "Coincidencias",
                requerido: true,
                placeholder: "0"
            },
            "Coordinacion con Upc": {
                tipo: "numero",
                unidad: "Coordinaciones",
                requerido: true,
                placeholder: "0"
            },
            "Combustibles": {
                tipo: "detalle_combustibles",
                unidad: "Galones",
                requerido: true,
                placeholder: "0.00",
                step: "0.01",
                campos: {
                    "total_combustibles": { label: "TOTAL COMBUSTIBLES (GL)", tipo: "numero", requerido: true, placeholder: "0.00", step: "0.01" },
                    "diesel": { label: "DIESEL (GL)", tipo: "numero", requerido: false, placeholder: "0.00", step: "0.01" },
                    "gasolina": { label: "GASOLINA (GL)", tipo: "numero", requerido: false, placeholder: "0.00", step: "0.01" },
                    "gasolina_pesca": { label: "GASOLINA DE PESCA (GL)", tipo: "numero", requerido: false, placeholder: "0.00", step: "0.01" }
                },
                validaciones: [
                    {
                        nombre: "Suma de tipos de combustibles vs total",
                        descripcion: "La suma de DIESEL + GASOLINA + GASOLINA DE PESCA debe ser igual al TOTAL COMBUSTIBLES",
                        validar: function(campos) {
                            const sumaTiposCombustibles = 
                                parseFloat(campos.diesel || 0) +
                                parseFloat(campos.gasolina || 0) +
                                parseFloat(campos.gasolina_pesca || 0);
                            
                            const totalCombustibles = parseFloat(campos.total_combustibles || 0);
                            return sumaTiposCombustibles === totalCombustibles;
                        }
                    }
                ]
            },
            "Coordinaciones Realizadas": {
                tipo: "numero",
                unidad: "Coordinaciones",
                requerido: true,
                placeholder: "0"
            },
            "Deposito de Dinero Real": {
                tipo: "numero",
                unidad: "Depósitos",
                requerido: true,
                placeholder: "0"
            },
            "Destiladores": {
                tipo: "numero",
                unidad: "Destiladores",
                requerido: true,
                placeholder: "0"
            },
            "Destruccion de Dinero Falso": {
                tipo: "numero",
                unidad: "Cantidad",
                requerido: true,
                placeholder: "0"
            },
            "Dinero en Cuentas Bancarias ($)": {
                tipo: "numero",
                unidad: "Dólares",
                requerido: true,
                placeholder: "0.00",
                step: "0.01"
            },
            "Disposicion Final de Armas de Fuego": {
                tipo: "numero",
                unidad: "Armas",
                requerido: true,
                placeholder: "0"
            },
            "Disposicion Final de Automotores": {
                tipo: "numero",
                unidad: "Vehículos",
                requerido: true,
                placeholder: "0"
            },
            "Disposicion Final de Bienes Muebles E Inmuebles": {
                tipo: "numero",
                unidad: "Bienes",
                requerido: true,
                placeholder: "0"
            },
            "Disposicion Final de Municiones": {
                tipo: "numero",
                unidad: "Municiones",
                requerido: true,
                placeholder: "0"
            },
            "Dolares Americanos ($)": {
                tipo: "numero",
                unidad: "Dólares",
                requerido: true,
                placeholder: "0.00",
                step: "0.01"
            },
            "Dolares Falsos ($)": {
                tipo: "numero",
                unidad: "Dólares",
                requerido: true,
                placeholder: "0.00",
                step: "0.01"
            },
            "Drogas": {
                tipo: "detalle_drogas",
                unidad: "Gramos",
                requerido: true,
                placeholder: "0.00",
                step: "0.01",
                campos: {
                    "total_drogas_gr": { label: "TOTAL DROGA (GR)", tipo: "numero", requerido: false, placeholder: "0.00", step: "0.01" },
                    "cocaina_gr": { label: "COCAINA (GR)", tipo: "numero", requerido: false, placeholder: "0.00", step: "0.01" },
                    "heroina_gr": { label: "HEROINA (GR)", tipo: "numero", requerido: false, placeholder: "0.00", step: "0.01" },
                    "marihuana_gr": { label: "MARIHUANA (GR)", tipo: "numero", requerido: false, placeholder: "0.00", step: "0.01" },
                    "sinteticas_gr": { label: "SINTETICAS (GR)", tipo: "numero", requerido: false, placeholder: "0.00", step: "0.01" },
                    "pasta_base_cocaina_gr": { label: "PASTA BASE DE COCAINA (GR)", tipo: "numero", requerido: false, placeholder: "0.00", step: "0.01" },
                    "otras_drogas_gr": { label: "OTRAS DROGAS (GR)", tipo: "numero", requerido: false, placeholder: "0.00", step: "0.01" }
                },
                validaciones: [
                    {
                        nombre: "Suma de tipos de drogas vs total",
                        descripcion: "La suma de COCAINA + HEROINA + MARIHUANA + SINTETICAS + PASTA BASE DE COCAINA + OTRAS DROGAS debe ser igual al TOTAL DROGA",
                        validar: function(campos) {
                            const sumaTiposDrogas = 
                                parseFloat(campos.cocaina_gr || 0) +
                                parseFloat(campos.heroina_gr || 0) +
                                parseFloat(campos.marihuana_gr || 0) +
                                parseFloat(campos.sinteticas_gr || 0) +
                                parseFloat(campos.pasta_base_cocaina_gr || 0) +
                                parseFloat(campos.otras_drogas_gr || 0);
                            
                            const totalDrogas = parseFloat(campos.total_drogas_gr || 0);
                            return sumaTiposDrogas === totalDrogas;
                        }
                    }
                ]
            },
            "Embarcaciones (Und)": {
                tipo: "numero",
                unidad: "Unidades",
                requerido: true,
                placeholder: "0"
            },
            "Fauna Silvestre (Und)": {
                tipo: "numero",
                unidad: "Unidades",
                requerido: true,
                placeholder: "0"
            },
            "Fulminantes (Und)": {
                tipo: "numero",
                unidad: "Unidades",
                requerido: true,
                placeholder: "0"
            },
            "Glp (15 - 45 Kg) (Und)": {
                tipo: "numero",
                unidad: "Unidades",
                requerido: true,
                placeholder: "0"
            },
            "Granadas (Und)": {
                tipo: "numero",
                unidad: "Unidades",
                requerido: true,
                placeholder: "0"
            },
            "Indique El Porcentaje de Cumplimiento de La Investigacion Previa (%)": {
                tipo: "numero",
                unidad: "Porcentaje",
                requerido: true,
                placeholder: "0",
                min: "0",
                max: "100"
            },
            "Informacion Relevante (Sms-Audios)": {
                tipo: "numero",
                unidad: "Elementos",
                requerido: true,
                placeholder: "0"
            },
            "Informes de Analisis": {
                tipo: "numero",
                unidad: "Informes",
                requerido: true,
                placeholder: "0"
            },
            "Investigaciones Previas Aperturadas": {
                tipo: "numero",
                unidad: "Investigaciones",
                requerido: true,
                placeholder: "0"
            },
            "Laboratorio_Procesamiento_Droga": {
                tipo: "numero",
                unidad: "Laboratorios",
                requerido: true,
                placeholder: "0"
            },
            "Madera (M3)": {
                tipo: "numero",
                unidad: "Metros Cúbicos",
                requerido: true,
                placeholder: "0.00",
                step: "0.01"
            },
            "Maquinaria Pesada (Und)": {
                tipo: "numero",
                unidad: "Unidades",
                requerido: true,
                placeholder: "0"
            },
            "Material Aurifero (Kg)": {
                tipo: "numero",
                unidad: "Kilogramos",
                requerido: true,
                placeholder: "0.00",
                step: "0.01"
            },
            "Mecha Lenta Cordon Detonante (Metros)": {
                tipo: "numero",
                unidad: "Metros",
                requerido: true,
                placeholder: "0.00",
                step: "0.01"
            },
            "Minas Detonantes (Und)": {
                tipo: "numero",
                unidad: "Unidades",
                requerido: true,
                placeholder: "0"
            },
            "Motores F. Borda (Und)": {
                tipo: "numero",
                unidad: "Unidades",
                requerido: true,
                placeholder: "0"
            },
            "Municiones (Und)": {
                tipo: "numero",
                unidad: "Unidades",
                requerido: true,
                placeholder: "0"
            },
            "Niños, Niñas y Adolescentes Rescatados": {
                tipo: "numero",
                unidad: "Personas",
                requerido: true,
                placeholder: "0"
            },
            "Nitrato de Amonio (Kg)": {
                tipo: "numero",
                unidad: "Kilogramos",
                requerido: true,
                placeholder: "0.00",
                step: "0.01"
            },
            "Numero de La Investigacion Previa Aperturada": {
                tipo: "texto",
                unidad: "Número",
                requerido: true,
                placeholder: "Ej: IP-2024-001"
            },
            "Operativos de Traslado": {
                tipo: "numero",
                unidad: "Operativos",
                requerido: true,
                placeholder: "0"
            },
            "Ordenes Judiciales de Analisis Telefonico Cumplidas": {
                tipo: "numero",
                unidad: "Órdenes",
                requerido: true,
                placeholder: "0"
            },
            "Patrimonio Cultural (Und)": {
                tipo: "numero",
                unidad: "Unidades",
                requerido: true,
                placeholder: "0"
            },
            "Pentolita (Kg)": {
                tipo: "numero",
                unidad: "Kilogramos",
                requerido: true,
                placeholder: "0.00",
                step: "0.01"
            },
            "Perdigones (Und)": {
                tipo: "numero",
                unidad: "Unidades",
                requerido: true,
                placeholder: "0"
            },
            "Perforaciones": {
                tipo: "numero",
                unidad: "Perforaciones",
                requerido: true,
                placeholder: "0"
            },
            "Pericias Accidentologia Vial": {
                tipo: "numero",
                unidad: "Pericias",
                requerido: true,
                placeholder: "0"
            },
            "Pericias Criminalistica": {
                tipo: "numero",
                unidad: "Pericias",
                requerido: true,
                placeholder: "0"
            },
            "Pericias Medicina Legal": {
                tipo: "numero",
                unidad: "Pericias",
                requerido: true,
                placeholder: "0"
            },
            "Personas Protegidas": {
                tipo: "numero",
                unidad: "Personas",
                requerido: true,
                placeholder: "0"
            },
            "Pirotecnia (Und)": {
                tipo: "numero",
                unidad: "Unidades",
                requerido: true,
                placeholder: "0"
            },
            "Piscinas Artesanales de Cldh": {
                tipo: "numero",
                unidad: "Piscinas",
                requerido: true,
                placeholder: "0"
            },
            "Polvora (Kg)": {
                tipo: "numero",
                unidad: "Kilogramos",
                requerido: true,
                placeholder: "0.00",
                step: "0.01"
            },
            "Productos Agricolas (Bultos)": {
                tipo: "numero",
                unidad: "Bultos",
                requerido: true,
                placeholder: "0"
            },
            "Quimicos Liquidos (Lt)": {
                tipo: "numero",
                unidad: "Litros",
                requerido: true,
                placeholder: "0.00",
                step: "0.01"
            },
            "Quimicos Solidos (Kg)": {
                tipo: "numero",
                unidad: "Kilogramos",
                requerido: true,
                placeholder: "0.00",
                step: "0.01"
            },
            "Registro de Medidas Cautelares": {
                tipo: "numero",
                unidad: "Registros",
                requerido: true,
                placeholder: "0"
            },
            "Reportes Telefonicos Entregados": {
                tipo: "numero",
                unidad: "Reportes",
                requerido: true,
                placeholder: "0"
            },
            "Semovientes (Und)": {
                tipo: "numero",
                unidad: "Unidades",
                requerido: true,
                placeholder: "0"
            },
            "Subzonas Intervenidas": {
                tipo: "numero",
                unidad: "Subzonas",
                requerido: true,
                placeholder: "0"
            },
            "Tacos de Dinamita (Und)": {
                tipo: "numero",
                unidad: "Unidades",
                requerido: true,
                placeholder: "0"
            },
            "Traslado de Indicios Bajo Pedido de Autoridad Competente": {
                tipo: "numero",
                unidad: "Traslados",
                requerido: true,
                placeholder: "0"
            },
            "Valor de los Bienes Inmuebles Incautados": {
                tipo: "numero",
                unidad: "Dólares",
                requerido: true,
                placeholder: "0.00",
                step: "0.01"
            },
            "Vehículos": {
                tipo: "detalle_vehiculos",
                unidad: "Vehículos",
                requerido: true,
                placeholder: "0",
                campos: {
                    "total_vehiculos": { label: "TOTAL VEHÍCULOS", tipo: "numero", requerido: false, placeholder: "0" },
                    "carros_aprehendidos": { label: "CARROS APREHENDIDOS", tipo: "numero", requerido: false, placeholder: "0" },
                    "carros_recuperados": { label: "CARROS RECUPERADOS", tipo: "numero", requerido: false, placeholder: "0" },
                    "motos_aprehendidas": { label: "MOTOS APREHENDIDAS", tipo: "numero", requerido: false, placeholder: "0" },
                    "motos_recuperadas": { label: "MOTOS RECUPERADAS", tipo: "numero", requerido: false, placeholder: "0" }
                },
                validaciones: [
                    {
                        nombre: "Suma de tipos de vehículos vs total",
                        descripcion: "La suma de CARROS APREHENDIDOS + CARROS RECUPERADOS + MOTOS APREHENDIDAS + MOTOS RECUPERADAS debe ser igual al TOTAL VEHÍCULOS",
                        validar: function(campos) {
                            const sumaTiposVehiculos = 
                                parseFloat(campos.carros_aprehendidos || 0) +
                                parseFloat(campos.carros_recuperados || 0) +
                                parseFloat(campos.motos_aprehendidas || 0) +
                                parseFloat(campos.motos_recuperadas || 0);
                            
                            const totalVehiculos = parseFloat(campos.total_vehiculos || 0);
                            return sumaTiposVehiculos === totalVehiculos;
                        }
                    }
                ]
            },
            "Victimas Liberadas": {
                tipo: "numero",
                unidad: "Personas",
                requerido: true,
                placeholder: "0"
            },
            "Víctimas Rescatadas": {
                tipo: "numero",
                unidad: "Personas",
                requerido: true,
                placeholder: "0"
            },
            "Varios": {
                tipo: "texto",
                unidad: "Descripción",
                requerido: true,
                placeholder: "Describa los elementos varios incautados..."
            }
        };

        // ========== VARIABLES GLOBALES ==========
        let detenidoCount = 0;
        let autoGeneratedDetentions = 0;
        let map;
        let marker;

        // ========== FUNCIONES PARA NOTIFICACIONES ==========
        function mostrarNotificacion(mensaje, tipo = 'info', tiempo = 5000) {
            const notificationArea = document.getElementById('notificationArea');
            const notification = document.createElement('div');
            notification.className = `notification ${tipo}`;
            
            let icono = 'ℹ️';
            if (tipo === 'success') icono = '✅';
            if (tipo === 'error') icono = '❌';
            if (tipo === 'warning') icono = '⚠️';
            
            notification.innerHTML = `
                <span>${icono}</span>
                <span>${mensaje}</span>
                <button class="notification-close">×</button>
            `;
            
            notificationArea.appendChild(notification);
            
            setTimeout(() => notification.classList.add('show'), 100);
            
            notification.querySelector('.notification-close').addEventListener('click', () => {
                notification.classList.remove('show');
                setTimeout(() => notification.remove(), 300);
            });
            
            if (tiempo > 0) {
                setTimeout(() => {
                    if (notification.parentNode) {
                        notification.classList.remove('show');
                        setTimeout(() => notification.remove(), 300);
                    }
                }, tiempo);
            }
        }

        // ========== FUNCIONES DEL MAPA ==========
        function inicializarMapa() {
            // Inicializar el mapa centrado en Ecuador
            map = L.map('map').setView([-1.8312, -78.1834], 6);
            
            // Agregar capa de tiles de OpenStreetMap
            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                attribution: '© OpenStreetMap contributors'
            }).addTo(map);

            // Agregar marcador inicial
            marker = L.marker([-1.8312, -78.1834], {
                draggable: true
            }).addTo(map);

            // Evento cuando se mueve el marcador
            marker.on('dragend', function(e) {
                const latlng = marker.getLatLng();
                actualizarCoordenadasDesdeMapa(latlng.lat, latlng.lng);
                obtenerDireccionDesdeCoordenadas(latlng.lat, latlng.lng);
            });

            // Evento cuando se hace clic en el mapa
            map.on('click', function(e) {
                marker.setLatLng(e.latlng);
                actualizarCoordenadasDesdeMapa(e.latlng.lat, e.latlng.lng);
                obtenerDireccionDesdeCoordenadas(e.latlng.lat, e.latlng.lng);
            });

            mostrarNotificacion('Mapa cargado correctamente. Haga clic en cualquier ubicación o busque una dirección.', 'info', 5000);
        }

        function mostrarMapa() {
            const mapContainer = document.getElementById('mapContainer');
            mapContainer.style.display = 'block';
            
            // Inicializar el mapa si no está inicializado
            if (!map) {
                setTimeout(() => {
                    inicializarMapa();
                }, 100);
            }
            
            // Ajustar el tamaño del mapa
            setTimeout(() => {
                map.invalidateSize();
            }, 300);
        }

        function ocultarMapa() {
            document.getElementById('mapContainer').style.display = 'none';
        }

        function actualizarCoordenadasDesdeMapa(lat, lng) {
            document.getElementById('latitud').value = lat.toString().replace('.', ',');
            document.getElementById('longitud').value = lng.toString().replace('.', ',');
            
            validarCoordenadas();
            actualizarBarraProgreso();
            
            // Mostrar información de la ubicación seleccionada
            document.getElementById('selectedLocationInfo').style.display = 'block';
            document.getElementById('selectedCoords').textContent = `${lat.toFixed(6)}, ${lng.toFixed(6)}`;
        }

        async function obtenerDireccionDesdeCoordenadas(lat, lng) {
            const addressElement = document.getElementById('selectedAddress');
            const direccionInput = document.getElementById('direccion');
            
            addressElement.textContent = 'Obteniendo dirección...';
            direccionInput.value = 'Obteniendo dirección...';
            
            try {
                const response = await fetch(`https://nominatim.openstreetmap.org/reverse?format=json&lat=${lat}&lon=${lng}&zoom=18&addressdetails=1`);
                const data = await response.json();
                
                if (data && data.display_name) {
                    const direccion = data.display_name;
                    addressElement.textContent = direccion;
                    direccionInput.value = direccion;
                    
                    // Marcar el campo de dirección como válido
                    direccionInput.classList.add('auto-filled');
                } else {
                    addressElement.textContent = 'Dirección no disponible';
                    direccionInput.value = 'Dirección no disponible';
                }
            } catch (error) {
                console.error('Error al obtener dirección:', error);
                addressElement.textContent = 'Error al obtener dirección';
                direccionInput.value = 'Error al obtener dirección';
            }
        }

        async function buscarDireccion(query) {
            const resultsContainer = document.getElementById('mapSearchResults');
            resultsContainer.innerHTML = '';
            
            if (!query || query.length < 3) {
                resultsContainer.style.display = 'none';
                return;
            }
            
            try {
                const response = await fetch(`https://nominatim.openstreetmap.org/search?format=json&q=${encodeURIComponent(query)}&limit=5&countrycodes=ec`);
                const data = await response.json();
                
                if (data && data.length > 0) {
                    data.forEach(result => {
                        const div = document.createElement('div');
                        div.className = 'map-search-result';
                        div.textContent = result.display_name;
                        div.addEventListener('click', function() {
                            const lat = parseFloat(result.lat);
                            const lon = parseFloat(result.lon);
                            
                            map.setView([lat, lon], 16);
                            marker.setLatLng([lat, lon]);
                            actualizarCoordenadasDesdeMapa(lat, lon);
                            obtenerDireccionDesdeCoordenadas(lat, lon);
                            
                            resultsContainer.style.display = 'none';
                            document.getElementById('mapSearch').value = result.display_name;
                        });
                        resultsContainer.appendChild(div);
                    });
                    resultsContainer.style.display = 'block';
                } else {
                    resultsContainer.style.display = 'none';
                }
            } catch (error) {
                console.error('Error en búsqueda de dirección:', error);
                resultsContainer.style.display = 'none';
            }
        }

        // ========== FUNCIONES PARA VARIABLES DE PRODUCTIVIDAD ==========
        function gestionarVariableProductividad() {
            const variableSeleccionada = document.getElementById('variablesProductividad').value;
            const container = document.getElementById('variableSectionsContainer');
            const validationSummary = document.getElementById('validationSummary');
            
            // Limpiar contenedor
            container.innerHTML = '';
            validationSummary.style.display = 'none';
            
            if (!variableSeleccionada) {
                // Ocultar sección de detenidos si no hay variable seleccionada
                document.getElementById('seccionDetenidos').style.display = 'none';
                return;
            }
            
            const configVariable = variablesProductividad[variableSeleccionada];
            if (!configVariable) {
                mostrarNotificacion('Error: Configuración no encontrada para esta variable', 'error', 3000);
                return;
            }
            
            // Crear sección para la variable seleccionada
            const section = document.createElement('div');
            section.className = 'variable-section active';
            section.id = `section-${variableSeleccionada.replace(/[^a-zA-Z0-9]/g, '-')}`;
            
            let inputHTML = '';
            
            if (configVariable.tipo === 'numero' || configVariable.tipo === 'texto') {
                const fieldId = `valor_${variableSeleccionada.replace(/[^a-zA-Z0-9]/g, '_')}`;
                
                if (configVariable.tipo === 'numero') {
                    inputHTML = `
                        <div class="form-grid">
                            <div class="form-group">
                                <label for="${fieldId}" class="required">Cantidad de ${variableSeleccionada}:</label>
                                <div class="variable-input-container">
                                    <input type="number" 
                                           id="${fieldId}" 
                                           name="${fieldId}"
                                           class="variable-input"
                                           ${configVariable.requerido ? 'required' : ''}
                                           min="${configVariable.min || '0'}"
                                           max="${configVariable.max || ''}"
                                           step="${configVariable.step || '1'}"
                                           placeholder="${configVariable.placeholder}"
                                           data-variable="${variableSeleccionada}">
                                    <span class="variable-unit">${configVariable.unidad}</span>
                                </div>
                                <div class="field-error" id="${fieldId}Error">Este campo es requerido</div>
                                <small class="hint">Ingrese la cantidad de ${variableSeleccionada.toLowerCase()} en ${configVariable.unidad.toLowerCase()}</small>
                            </div>
                        </div>
                    `;
                } else if (configVariable.tipo === 'texto') {
                    inputHTML = `
                        <div class="form-grid">
                            <div class="form-group form-group-full">
                                <label for="${fieldId}" class="required">${variableSeleccionada}:</label>
                                <input type="text" 
                                       id="${fieldId}" 
                                       name="${fieldId}"
                                       class="variable-input"
                                       ${configVariable.requerido ? 'required' : ''}
                                       placeholder="${configVariable.placeholder}"
                                       data-variable="${variableSeleccionada}">
                                <div class="field-error" id="${fieldId}Error">Este campo es requerido</div>
                                <small class="hint">${configVariable.unidad}: ${configVariable.placeholder}</small>
                            </div>
                        </div>
                    `;
                }
            } else if (configVariable.tipo.startsWith('detalle_')) {
                // Para variables con campos detallados
                inputHTML = generarCamposDetallados(variableSeleccionada, configVariable);
            }
            
            section.innerHTML = `
                <h3 class="subsection-title">📊 ${variableSeleccionada}</h3>
                ${inputHTML}
            `;
            
            container.appendChild(section);
            
            // Agregar event listeners para validación
            if (configVariable.tipo === 'numero' || configVariable.tipo === 'texto') {
                const fieldId = `valor_${variableSeleccionada.replace(/[^a-zA-Z0-9]/g, '_')}`;
                const inputElement = document.getElementById(fieldId);
                if (inputElement) {
                    inputElement.addEventListener('input', function() {
                        validarVariableProductividad(this);
                        actualizarBarraProgreso();
                    });
                    
                    inputElement.addEventListener('blur', function() {
                        validarVariableProductividad(this);
                    });
                }
            } else if (configVariable.tipo.startsWith('detalle_')) {
                // Agregar event listeners para campos detallados
                agregarEventListenersCamposDetallados(variableSeleccionada, configVariable);
            }
            
            // Scroll a la sección
            setTimeout(() => {
                section.scrollIntoView({ behavior: 'smooth', block: 'start' });
            }, 300);
            
            actualizarBarraProgreso();
        }

        function generarCamposDetallados(variableSeleccionada, configVariable) {
            let camposHTML = '';
            
            if (configVariable.campos) {
                camposHTML = '<div class="form-grid">';
                
                Object.entries(configVariable.campos).forEach(([campoId, configCampo]) => {
                    const campoCompletoId = `${variableSeleccionada.replace(/[^a-zA-Z0-9]/g, '_')}_${campoId}`;
                    
                    if (configCampo.tipo === 'numero') {
                        camposHTML += `
                            <div class="form-group">
                                <label for="${campoCompletoId}" ${configCampo.requerido ? 'class="required"' : ''}>${configCampo.label}:</label>
                                <div class="variable-input-container">
                                    <input type="number" 
                                           id="${campoCompletoId}" 
                                           name="${campoCompletoId}"
                                           class="variable-input"
                                           ${configCampo.requerido ? 'required' : ''}
                                           min="0"
                                           step="${configCampo.step || '1'}"
                                           placeholder="${configCampo.placeholder}"
                                           data-variable="${variableSeleccionada}"
                                           data-campo="${campoId}">
                                    <span class="variable-unit">${configCampo.step === '0.01' ? (variableSeleccionada === 'Combustibles' ? 'Galones' : 'Gramos') : 'Unidades'}</span>
                                </div>
                                <div class="field-error" id="${campoCompletoId}Error">Este campo es requerido</div>
                            </div>
                        `;
                    }
                });
                
                camposHTML += '</div>';
                
                // Agregar información de cálculo automático
                if (configVariable.validaciones && configVariable.validaciones.length > 0) {
                    camposHTML += `
                        <div class="auto-calculation">
                            <h4>📝 Información de Validación</h4>
                            <p>Los campos se validarán automáticamente según las siguientes reglas:</p>
                            <ul>
                                ${configVariable.validaciones.map(v => `<li>${v.descripcion}</li>`).join('')}
                            </ul>
                        </div>
                    `;
                }
            }
            
            return camposHTML;
        }

        function agregarEventListenersCamposDetallados(variableSeleccionada, configVariable) {
            if (configVariable.campos) {
                Object.entries(configVariable.campos).forEach(([campoId, configCampo]) => {
                    const campoCompletoId = `${variableSeleccionada.replace(/[^a-zA-Z0-9]/g, '_')}_${campoId}`;
                    const inputElement = document.getElementById(campoCompletoId);
                    
                    if (inputElement) {
                        inputElement.addEventListener('input', function() {
                            validarCampoDetallado(this);
                            ejecutarValidacionesVariables(variableSeleccionada);
                            actualizarBarraProgreso();
                            
                            // Si es la variable "Aprehendidos/Detenidos", gestionar detenidos
                            if (variableSeleccionada === "Aprehendidos/Detenidos") {
                                gestionarDetenidosSegunSuma();
                            }
                        });
                        
                        inputElement.addEventListener('blur', function() {
                            validarCampoDetallado(this);
                            ejecutarValidacionesVariables(variableSeleccionada);
                            actualizarBarraProgreso();
                            
                            // Si es la variable "Aprehendidos/Detenidos", gestionar detenidos
                            if (variableSeleccionada === "Aprehendidos/Detenidos") {
                                gestionarDetenidosSegunSuma();
                            }
                        });
                    }
                });
            }
        }

        // ========== FUNCIÓN PARA GESTIONAR DETENIDOS SEGÚN SUMA ==========
        function gestionarDetenidosSegunSuma() {
            const variableSeleccionada = document.getElementById('variablesProductividad').value;
            
            // Solo proceder si la variable seleccionada es "Aprehendidos/Detenidos"
            if (variableSeleccionada !== "Aprehendidos/Detenidos") {
                return;
            }
            
            // Obtener valores de los campos
            const cantidadAprehendidos = parseInt(document.getElementById('Aprehendidos_Detenidos_cantidad_aprehendidos').value) || 0;
            const cantidadDetenidos = parseInt(document.getElementById('Aprehendidos_Detenidos_cantidad_detenidos').value) || 0;
            
            // Calcular suma total
            const totalDetenidos = cantidadAprehendidos + cantidadDetenidos;
            
            // Mostrar u ocultar sección de detenidos
            const seccionDetenidos = document.getElementById('seccionDetenidos');
            
            if (totalDetenidos > 0) {
                seccionDetenidos.style.display = 'block';
                
                // Ajustar el número de formularios de detenidos según la suma
                ajustarFormulariosDetenidos(totalDetenidos);
                
                // Hacer scroll a la sección de detenidos
                setTimeout(() => {
                    seccionDetenidos.scrollIntoView({ behavior: 'smooth', block: 'start' });
                }, 300);
                
                mostrarNotificacion(`Se han activado ${totalDetenidos} formulario(s) de detenido(s)`, 'info', 3000);
            } else {
                seccionDetenidos.style.display = 'none';
                // Limpiar todos los detenidos si la suma es 0
                document.getElementById('detenidosContainer').innerHTML = '';
                detenidoCount = 0;
                actualizarContadorDetenidos();
            }
            
            actualizarBarraProgreso();
        }

        function ajustarFormulariosDetenidos(totalRequerido) {
            const container = document.getElementById('detenidosContainer');
            const detenidosActuales = container.querySelectorAll('.detenido-container').length;
            
            // Si necesitamos más detenidos de los que hay actualmente
            if (totalRequerido > detenidosActuales) {
                const cantidadAAgregar = totalRequerido - detenidosActuales;
                for (let i = 0; i < cantidadAAgregar; i++) {
                    agregarDetenido();
                }
            }
            // Si necesitamos menos detenidos de los que hay actualmente
            else if (totalRequerido < detenidosActuales) {
                const cantidadAEliminar = detenidosActuales - totalRequerido;
                for (let i = 0; i < cantidadAEliminar; i++) {
                    const idAEliminar = detenidoCount;
                    eliminarDetenido(idAEliminar);
                }
            }
            
            // Deshabilitar el botón de agregar detenido manualmente
            // NOTA: El botón ya fue eliminado, pero mantenemos la lógica por si acaso
            const btnAgregar = document.getElementById('btnAgregarDetenido');
            if (btnAgregar) {
                btnAgregar.disabled = true;
                btnAgregar.style.opacity = '0.5';
                btnAgregar.style.cursor = 'not-allowed';
            }
            
            // Actualizar el texto del contador
            document.getElementById('totalDetenidos').textContent = totalRequerido;
        }

        function validarCampoDetallado(inputElement) {
            const errorElement = document.getElementById(inputElement.id + 'Error');
            const valor = inputElement.value.trim();
            const esRequerido = inputElement.hasAttribute('required');
            
            let esValido = true;
            
            if (esRequerido && !valor) {
                esValido = false;
            } else if (valor && inputElement.type === 'number') {
                const numero = parseFloat(valor);
                if (isNaN(numero) || numero < 0) {
                    esValido = false;
                }
            }
            
            if (esValido) {
                inputElement.classList.remove('invalid');
                inputElement.classList.add('valid');
                if (errorElement) errorElement.style.display = 'none';
            } else {
                inputElement.classList.remove('valid');
                inputElement.classList.add('invalid');
                if (errorElement) errorElement.style.display = 'block';
            }
            
            return esValido;
        }

        function validarVariableProductividad(inputElement) {
            const errorElement = document.getElementById(inputElement.id + 'Error');
            const valor = inputElement.value.trim();
            const configVariable = variablesProductividad[inputElement.dataset.variable];
            
            if (!configVariable) return false;
            
            let esValido = true;
            
            if (configVariable.requerido && !valor) {
                esValido = false;
            } else if (configVariable.tipo === 'numero' && valor) {
                const numero = parseFloat(valor);
                if (isNaN(numero) || numero < 0) {
                    esValido = false;
                }
                if (configVariable.min && numero < parseFloat(configVariable.min)) {
                    esValido = false;
                }
                if (configVariable.max && numero > parseFloat(configVariable.max)) {
                    esValido = false;
                }
            }
            
            if (esValido) {
                inputElement.classList.remove('invalid');
                inputElement.classList.add('valid');
                if (errorElement) errorElement.style.display = 'none';
            } else {
                inputElement.classList.remove('valid');
                inputElement.classList.add('invalid');
                if (errorElement) errorElement.style.display = 'block';
            }
            
            return esValido;
        }

        function ejecutarValidacionesVariables(variableSeleccionada) {
            const configVariable = variablesProductividad[variableSeleccionada];
            const validationSummary = document.getElementById('validationSummary');
            const validationSummaryContent = document.getElementById('validationSummaryContent');
            
            if (!configVariable || !configVariable.validaciones) {
                validationSummary.style.display = 'none';
                return true;
            }
            
            // Obtener valores de todos los campos
            const valoresCampos = {};
            Object.keys(configVariable.campos).forEach(campoId => {
                const campoCompletoId = `${variableSeleccionada.replace(/[^a-zA-Z0-9]/g, '_')}_${campoId}`;
                const inputElement = document.getElementById(campoCompletoId);
                if (inputElement) {
                    valoresCampos[campoId] = inputElement.value;
                }
            });
            
            // Ejecutar validaciones
            let todasValidas = true;
            let htmlValidaciones = '';
            
            configVariable.validaciones.forEach(validacion => {
                const esValida = validacion.validar(valoresCampos);
                todasValidas = todasValidas && esValida;
                
                htmlValidaciones += `
                    <div class="validation-summary-item ${esValida ? 'valid' : 'invalid'}">
                        ${esValida ? '✅' : '❌'} ${validacion.descripcion}
                    </div>
                `;
            });
            
            // Mostrar resumen de validaciones
            validationSummaryContent.innerHTML = htmlValidaciones;
            validationSummary.style.display = 'block';
            validationSummary.className = `validation-summary ${todasValidas ? 'success' : 'error'}`;
            
            return todasValidas;
        }

        function validarTodasVariablesProductividad() {
            const variableSeleccionada = document.getElementById('variablesProductividad').value;
            if (!variableSeleccionada) {
                mostrarNotificacion('❌ Debe seleccionar una variable de productividad', 'error', 5000);
                return false;
            }
            
            const configVariable = variablesProductividad[variableSeleccionada];
            
            if (configVariable.tipo === 'numero' || configVariable.tipo === 'texto') {
                const fieldId = `valor_${variableSeleccionada.replace(/[^a-zA-Z0-9]/g, '_')}`;
                const inputElement = document.getElementById(fieldId);
                
                if (inputElement) {
                    return validarVariableProductividad(inputElement);
                }
            } else if (configVariable.tipo.startsWith('detalle_')) {
                // Validar todos los campos detallados
                if (configVariable.campos) {
                    let todosValidos = true;
                    Object.entries(configVariable.campos).forEach(([campoId, configCampo]) => {
                        const campoCompletoId = `${variableSeleccionada.replace(/[^a-zA-Z0-9]/g, '_')}_${campoId}`;
                        const inputElement = document.getElementById(campoCompletoId);
                        
                        if (inputElement && configCampo.requerido) {
                            if (!validarCampoDetallado(inputElement)) {
                                todosValidos = false;
                            }
                        }
                    });
                    
                    // Ejecutar validaciones específicas
                    const validacionesValidas = ejecutarValidacionesVariables(variableSeleccionada);
                    
                    return todosValidos && validacionesValidas;
                }
            }
            
            return false;
        }

        // ========== FUNCIONES PARA DETENIDOS ==========
        function agregarDetenido() {
            detenidoCount++;
            const container = document.getElementById('detenidosContainer');
            
            const detenidoDiv = document.createElement('div');
            detenidoDiv.className = 'detenido-container';
            detenidoDiv.id = `detenido-${detenidoCount}`;
            
            detenidoDiv.innerHTML = `
                <div class="detenido-header">
                    <div class="detenido-title">
                        <span class="detenido-count">${detenidoCount}</span>
                        Detenido #${detenidoCount}
                    </div>
                    <button type="button" class="remove-detenido" data-id="${detenidoCount}" style="display: none;">×</button>
                </div>
                
                <div class="form-grid">
                    <div class="form-group form-group-full">
                        <label for="nombresDetenido-${detenidoCount}" class="required">Apellidos y Nombres del Detenido:</label>
                        <input type="text" id="nombresDetenido-${detenidoCount}" name="nombresDetenido-${detenidoCount}" 
                               placeholder="Ej: PEREZ GONZALEZ JUAN CARLOS" required>
                        <div class="field-error" id="nombresDetenidoError-${detenidoCount}">Este campo es requerido</div>
                    </div>
                </div>
                
                <div class="form-grid">
                    <div class="form-group">
                        <label for="cedulaDetenido-${detenidoCount}" class="required">Cédula/Pasaporte del Detenido:</label>
                        <input type="text" id="cedulaDetenido-${detenidoCount}" name="cedulaDetenido-${detenidoCount}" 
                               placeholder="Ej: 1234567890 o AB123456" required>
                        <div class="field-error" id="cedulaDetenidoError-${detenidoCount}">Este campo es requerido</div>
                    </div>
                    
                    <div class="form-group">
                        <label for="edadDetenido-${detenidoCount}" class="required">Edad del Detenido:</label>
                        <input type="number" id="edadDetenido-${detenidoCount}" name="edadDetenido-${detenidoCount}" 
                               min="1" max="120" placeholder="Ej: 25" required>
                        <div class="field-error" id="edadDetenidoError-${detenidoCount}">La edad debe estar entre 1 y 120 años</div>
                    </div>
                </div>
                
                <div class="form-grid">
                    <div class="form-group">
                        <label for="nacionalidadDetenido-${detenidoCount}" class="required">Nacionalidad del Detenido:</label>
                        <input type="text" id="nacionalidadDetenido-${detenidoCount}" name="nacionalidadDetenido-${detenidoCount}" 
                               list="nacionalidadesList" placeholder="Ej: ECUATORIANA" required>
                        <datalist id="nacionalidadesList">
                            ${nacionalidades.map(n => `<option value="${n}">`).join('')}
                        </datalist>
                        <div class="field-error" id="nacionalidadDetenidoError-${detenidoCount}">Este campo es requerido</div>
                    </div>
                    
                    <div class="form-group">
                        <label for="cantidadDetenciones-${detenidoCount}">Cantidad de Detenciones del Detenido:</label>
                        <input type="number" id="cantidadDetenciones-${detenidoCount}" name="cantidadDetenciones-${detenidoCount}" 
                               min="0" placeholder="Ej: 2">
                        <small class="small-hint">Dejar en 0 si es la primera detención</small>
                    </div>
                </div>
                
                <div class="form-grid">
                    <div class="form-group">
                        <label for="resolucionFlagrancia-${detenidoCount}">Resolución de la Flagrancia del Detenido:</label>
                        <select id="resolucionFlagrancia-${detenidoCount}" name="resolucionFlagrancia-${detenidoCount}">
                            <option value="">Seleccione una resolución...</option>
                            ${resolucionesFlagrancia.map(resolucion => `<option value="${resolucion}">${resolucion}</option>`).join('')}
                        </select>
                    </div>
                    
                    <div class="form-group">
                        <label for="grupoDelictivo-${detenidoCount}">Grupo Delictivo al que Pertenece el Detenido:</label>
                        <select id="grupoDelictivo-${detenidoCount}" name="grupoDelictivo-${detenidoCount}">
                            <option value="">Seleccione un grupo...</option>
                            ${gruposDelictivos.map(g => `<option value="${g}">${g}</option>`).join('')}
                        </select>
                    </div>
                </div>
                
                <div class="form-grid">
                    <div class="form-group">
                        <label for="servidorPolicial-${detenidoCount}">¿El Detenido es Servidor Policial?</label>
                        <select id="servidorPolicial-${detenidoCount}" name="servidorPolicial-${detenidoCount}">
                            <option value="">Seleccione...</option>
                            <option value="SI">SÍ</option>
                            <option value="NO">NO</option>
                        </select>
                    </div>
                </div>
            `;
            
            container.appendChild(detenidoDiv);
            actualizarContadorDetenidos();
            
            // Agregar event listeners para validación
            agregarEventListenersDetenido(detenidoCount);
            
            // Hacer scroll al nuevo detenido
            setTimeout(() => {
                detenidoDiv.scrollIntoView({ behavior: 'smooth', block: 'start' });
            }, 300);
        }

        // Función para agregar event listeners a un detenido
        function agregarEventListenersDetenido(id) {
            const nombresInput = document.getElementById(`nombresDetenido-${id}`);
            const cedulaInput = document.getElementById(`cedulaDetenido-${id}`);
            const edadInput = document.getElementById(`edadDetenido-${id}`);
            const nacionalidadInput = document.getElementById(`nacionalidadDetenido-${id}`);
            
            nombresInput.addEventListener('blur', () => validarCampoRequerido(nombresInput, id, 'nombresDetenido'));
            cedulaInput.addEventListener('blur', () => validarCampoRequerido(cedulaInput, id, 'cedulaDetenido'));
            edadInput.addEventListener('blur', () => validarEdad(edadInput, id));
            nacionalidadInput.addEventListener('blur', () => validarCampoRequerido(nacionalidadInput, id, 'nacionalidadDetenido'));
            
            // Convertir a mayúsculas mientras se escribe
            nombresInput.addEventListener('input', function() {
                this.value = this.value.toUpperCase();
            });
            
            nacionalidadInput.addEventListener('input', function() {
                this.value = this.value.toUpperCase();
            });
        }

        // Función para validar campos requeridos
        function validarCampoRequerido(campo, id, tipo) {
            const errorElement = document.getElementById(`${tipo}Error-${id}`);
            
            if (campo.value.trim() === '') {
                campo.classList.remove('valid');
                campo.classList.add('invalid');
                errorElement.style.display = 'block';
                return false;
            } else {
                campo.classList.remove('invalid');
                campo.classList.add('valid');
                errorElement.style.display = 'none';
                return true;
            }
        }

        // Función para validar edad
        function validarEdad(campo, id) {
            const errorElement = document.getElementById(`edadDetenidoError-${id}`);
            const edad = parseInt(campo.value);
            
            if (isNaN(edad) || edad < 1 || edad > 120) {
                campo.classList.remove('valid');
                campo.classList.add('invalid');
                errorElement.style.display = 'block';
                return false;
            } else {
                campo.classList.remove('invalid');
                campo.classList.add('valid');
                errorElement.style.display = 'none';
                return true;
            }
        }

        // Función para eliminar un detenido
        function eliminarDetenido(id) {
            const detenidoDiv = document.getElementById(`detenido-${id}`);
            if (detenidoDiv) {
                detenidoDiv.remove();
                actualizarContadorDetenidos();
                mostrarNotificacion(`Detenido #${id} eliminado`, 'info', 3000);
            }
        }

        // Función para actualizar el contador de detenidos
        function actualizarContadorDetenidos() {
            const contenedores = document.querySelectorAll('.detenido-container');
            document.getElementById('totalDetenidos').textContent = contenedores.length;
            
            // Actualizar números de detenidos
            contenedores.forEach((contenedor, index) => {
                const countSpan = contenedor.querySelector('.detenido-count');
                const titleSpan = contenedor.querySelector('.detenido-title');
                
                if (countSpan) countSpan.textContent = index + 1;
                if (titleSpan) {
                    titleSpan.innerHTML = `<span class="detenido-count">${index + 1}</span> Detenido #${index + 1}`;
                }
            });
        }

        // Función para validar todos los detenidos
        function validarTodosDetenidos() {
            if (detenidoCount === 0) {
                return true; // No hay detenidos, pero no es un error
            }
            
            let todosValidos = true;
            
            for (let i = 1; i <= detenidoCount; i++) {
                const detenidoDiv = document.getElementById(`detenido-${i}`);
                if (!detenidoDiv) continue;
                
                const nombres = document.getElementById(`nombresDetenido-${i}`);
                const cedula = document.getElementById(`cedulaDetenido-${i}`);
                const edad = document.getElementById(`edadDetenido-${i}`);
                const nacionalidad = document.getElementById(`nacionalidadDetenido-${i}`);
                
                if (nombres && !validarCampoRequerido(nombres, i, 'nombresDetenido')) {
                    todosValidos = false;
                }
                
                if (cedula && !validarCampoRequerido(cedula, i, 'cedulaDetenido')) {
                    todosValidos = false;
                }
                
                if (edad && !validarEdad(edad, i)) {
                    todosValidos = false;
                }
                
                if (nacionalidad && !validarCampoRequerido(nacionalidad, i, 'nacionalidadDetenido')) {
                    todosValidos = false;
                }
            }
            
            return todosValidos;
        }

        // ========== FUNCIONES AUXILIARES ==========
        function actualizarUnidades() {
            const direccion = document.getElementById('direccionInterventora').value;
            const unidadSelect = document.getElementById('unidadInterventora');
            const unidadesStatus = document.getElementById('unidadesStatus');
            
            unidadSelect.innerHTML = '';
            unidadSelect.disabled = true;
            
            if (!direccion) {
                unidadSelect.innerHTML = '<option value="">Primero seleccione una dirección...</option>';
                unidadesStatus.textContent = '';
                return;
            }
            
            const unidades = datosUnidades[direccion];
            
            if (unidades && unidades.length > 0) {
                const defaultOption = document.createElement('option');
                defaultOption.value = '';
                defaultOption.textContent = `Seleccione una unidad de ${direccion}...`;
                unidadSelect.appendChild(defaultOption);
                
                unidades.forEach((unidad) => {
                    const option = document.createElement('option');
                    option.value = unidad;
                    option.textContent = unidad;
                    unidadSelect.appendChild(option);
                });
                
                unidadSelect.disabled = false;
                unidadesStatus.textContent = `✓ ${unidades.length} unidades disponibles`;
                unidadesStatus.className = 'data-status success';
            } else {
                unidadSelect.innerHTML = '<option value="">No hay unidades disponibles para esta dirección</option>';
                unidadesStatus.textContent = `✗ No se encontraron unidades para ${direccion}`;
                unidadesStatus.className = 'data-status error';
            }
            
            actualizarBarraProgreso();
        }

        function validarCampo(campo, errorElement) {
            const valor = campo.value.trim();
            
            if (campo.validity.valid && valor !== '') {
                campo.classList.remove('invalid');
                campo.classList.add('valid');
                if (errorElement) errorElement.style.display = 'none';
                return true;
            } else {
                campo.classList.remove('valid');
                campo.classList.add('invalid');
                if (errorElement) errorElement.style.display = 'block';
                return false;
            }
        }

        function validarCedula() {
            const cedula = document.getElementById('numeroCedula');
            const error = document.getElementById('cedulaError');
            const valor = cedula.value.trim();
            
            if (/^\d{10}$/.test(valor)) {
                cedula.classList.remove('invalid');
                cedula.classList.add('valid');
                error.style.display = 'none';
                
                clearTimeout(window.cedulaTimeout);
                window.cedulaTimeout = setTimeout(() => buscarPorCedula(valor), 800);
                return true;
            } else {
                cedula.classList.remove('valid');
                cedula.classList.add('invalid');
                error.style.display = 'block';
                document.getElementById('cedulaStatus').textContent = '';
                return false;
            }
        }

        // ========== BUSQUEDA DE DATOS POR CÉDULA ==========
        async function buscarPorCedula(cedula) {
            const cedulaStatus = document.getElementById('cedulaStatus');
            const loadingIcon = document.querySelector('.cedula-loading');
            
            if (cedula.length !== 10 || !/^\d{10}$/.test(cedula)) {
                cedulaStatus.textContent = 'Ingrese una cédula válida de 10 dígitos';
                cedulaStatus.className = 'data-status error';
                return;
            }
            
            loadingIcon.classList.add('active');
            cedulaStatus.textContent = 'Buscando datos del funcionario...';
            cedulaStatus.className = 'data-status loading';
            
            document.getElementById('grado').value = '';
            document.getElementById('apellidosNombres').value = '';
            document.getElementById('celular').value = '';
            document.getElementById('correoElectronico').value = '';
            
            document.getElementById('grado').classList.remove('auto-filled');
            document.getElementById('apellidosNombres').classList.remove('auto-filled');
            document.getElementById('celular').classList.remove('auto-filled');
            document.getElementById('correoElectronico').classList.remove('auto-filled');
            
            try {
                const response = await fetch(`${APPS_SCRIPT_URL_CEDULA}?cedula=${cedula}&timestamp=${Date.now()}`);
                
                if (response.ok) {
                    const data = await response.json();
                    
                    if (data.success) {
                        document.getElementById('grado').value = data.grado || '';
                        document.getElementById('apellidosNombres').value = data.nombres || '';
                        document.getElementById('celular').value = data.celular || '';
                        document.getElementById('correoElectronico').value = data.correo || '';
                        
                        document.getElementById('grado').classList.add('auto-filled');
                        document.getElementById('apellidosNombres').classList.add('auto-filled');
                        document.getElementById('celular').classList.add('auto-filled');
                        document.getElementById('correoElectronico').classList.add('auto-filled');
                        
                        validarCampo(document.getElementById('grado'), document.getElementById('gradoError'));
                        validarCampo(document.getElementById('apellidosNombres'), document.getElementById('nombresError'));
                        validarCampo(document.getElementById('celular'), document.getElementById('celularError'));
                        validarCampo(document.getElementById('correoElectronico'), document.getElementById('correoError'));
                        
                        cedulaStatus.textContent = '✓ Datos del funcionario cargados correctamente';
                        cedulaStatus.className = 'data-status success';
                        actualizarBarraProgreso();
                        
                    } else {
                        cedulaStatus.textContent = '✗ ' + (data.message || 'No se encontró el funcionario');
                        cedulaStatus.className = 'data-status error';
                    }
                } else {
                    throw new Error(`Error HTTP: ${response.status}`);
                }
                
            } catch (error) {
                console.error('Error:', error);
                cedulaStatus.textContent = '✗ Error de conexión con el servidor';
                cedulaStatus.className = 'data-status error';
            } finally {
                loadingIcon.classList.remove('active');
            }
        }

        // ========== BUSQUEDA DE DATOS DE SUBCIRCUITO ==========
        async function buscarSubcircuito(codigo) {
            const dataStatus = document.getElementById('dataStatus');
            const codigoBuscado = codigo.toUpperCase().trim();
            
            if (!codigoBuscado) {
                dataStatus.textContent = '';
                return;
            }
            
            dataStatus.textContent = 'Buscando subcircuito...';
            dataStatus.className = 'data-status loading';
            
            try {
                console.log('🔍 Buscando subcircuito:', codigoBuscado);
                
                const response = await fetch(`${APPS_SCRIPT_URL_SUBCIRCUITO}?codigo=${codigoBuscado}&timestamp=${Date.now()}`);
                
                if (response.ok) {
                    const data = await response.json();
                    console.log('📊 Respuesta del servidor:', data);
                    
                    if (data.success && data.data) {
                        document.getElementById('zona').value = data.data.zona || '';
                        document.getElementById('subzona').value = data.data.subzona || '';
                        document.getElementById('distrito').value = data.data.distrito || '';
                        document.getElementById('circuito').value = data.data.circuito || '';
                        document.getElementById('subcircuito').value = data.data.subcircuito || '';
                        
                        dataStatus.textContent = '✓ Datos cargados correctamente';
                        dataStatus.className = 'data-status success';
                        actualizarBarraProgreso();
                        return;
                    } else {
                        dataStatus.textContent = data.message || '✗ Código no encontrado';
                        dataStatus.className = 'data-status error';
                    }
                } else {
                    throw new Error(`Error HTTP: ${response.status}`);
                }
                
            } catch (error) {
                console.error('❌ Error en búsqueda de subcircuito:', error);
                dataStatus.textContent = '✗ Error de conexión con el servidor';
                dataStatus.className = 'data-status error';
            }
        }

        // ========== BÚSQUEDA DE DELITOS ==========
        function buscarDelitos(termino) {
            const delitoInput = document.getElementById('presuntoDelito');
            const resultados = document.getElementById('delitoSearchResults');
            
            resultados.innerHTML = '';
            
            if (!termino || termino.length < 2) {
                resultados.style.display = 'none';
                return;
            }
            
            const terminoBusqueda = termino.toUpperCase();
            const delitosFiltrados = listaDelitos.filter(delito => 
                delito.includes(terminoBusqueda)
            ).slice(0, 10);
            
            if (delitosFiltrados.length === 0) {
                resultados.style.display = 'none';
                return;
            }
            
            delitosFiltrados.forEach(delito => {
                const div = document.createElement('div');
                div.className = 'delito-search-result';
                div.textContent = delito;
                div.addEventListener('click', function() {
                    delitoInput.value = delito;
                    resultados.style.display = 'none';
                    validarDelito();
                });
                resultados.appendChild(div);
            });
            
            resultados.style.display = 'block';
        }

        function validarDelito() {
            const delitoInput = document.getElementById('presuntoDelito');
            const delitoError = document.getElementById('delitoError');
            
            if (delitoInput.value && listaDelitos.includes(delitoInput.value.toUpperCase())) {
                delitoError.style.display = 'none';
                delitoInput.classList.remove('invalid');
                delitoInput.classList.add('valid');
                return true;
            } else {
                delitoError.style.display = 'block';
                delitoInput.classList.remove('valid');
                delitoInput.classList.add('invalid');
                return false;
            }
        }

        // ========== OBTENER UBICACIÓN ==========
        function obtenerUbicacion() {
            const locationStatus = document.getElementById('locationStatus');
            const latitud = document.getElementById('latitud');
            const longitud = document.getElementById('longitud');
            const permissionHelp = document.getElementById('permissionHelp');
            
            locationStatus.innerHTML = '<span class="location-loading">🔄 Obteniendo ubicación...</span>';
            permissionHelp.style.display = 'none';
            
            if (!navigator.geolocation) {
                locationStatus.innerHTML = '<span class="location-error">❌ La geolocalización no es compatible con este navegador</span>';
                permissionHelp.style.display = 'block';
                return;
            }
            
            navigator.geolocation.getCurrentPosition(
                function(position) {
                    const lat = position.coords.latitude;
                    const lng = position.coords.longitude;
                    
                    latitud.value = lat.toString().replace('.', ',');
                    longitud.value = lng.toString().replace('.', ',');
                    
                    locationStatus.innerHTML = '<span class="location-success">✅ Ubicación obtenida correctamente</span>';
                    validarCoordenadas();
                    actualizarBarraProgreso();
                    
                    // Obtener dirección automáticamente
                    obtenerDireccionDesdeCoordenadas(lat, lng);
                },
                function(error) {
                    let mensaje = '';
                    switch(error.code) {
                        case error.PERMISSION_DENIED:
                            mensaje = '❌ Permiso de ubicación denegado';
                            permissionHelp.style.display = 'block';
                            break;
                        case error.POSITION_UNAVAILABLE:
                            mensaje = '❌ Información de ubicación no disponible';
                            permissionHelp.style.display = 'block';
                            break;
                        case error.TIMEOUT:
                            mensaje = '❌ Tiempo de espera agotado para obtener ubicación';
                            break;
                        default:
                            mensaje = '❌ Error desconocido al obtener ubicación';
                    }
                    locationStatus.innerHTML = `<span class="location-error">${mensaje}</span>`;
                },
                {
                    enableHighAccuracy: true,
                    timeout: 10000,
                    maximumAge: 60000
                }
            );
        }

        // ========== VALIDACIÓN DE COORDENADAS ==========
        function validarCoordenadas() {
            const latitud = document.getElementById('latitud');
            const longitud = document.getElementById('longitud');
            const latitudError = document.getElementById('latitudError');
            const longitudError = document.getElementById('longitudError');
            
            const patronLatitud = /^-?\d{1,2},\d+$/;
            const patronLongitud = /^-?\d{1,3},\d+$/;
            
            let valido = true;
            
            if (patronLatitud.test(latitud.value)) {
                latitud.classList.remove('invalid');
                latitud.classList.add('valid');
                latitudError.style.display = 'none';
            } else {
                latitud.classList.remove('valid');
                latitud.classList.add('invalid');
                latitudError.style.display = 'block';
                valido = false;
            }
            
            if (patronLongitud.test(longitud.value)) {
                longitud.classList.remove('invalid');
                longitud.classList.add('valid');
                longitudError.style.display = 'none';
            } else {
                longitud.classList.remove('valid');
                longitud.classList.add('invalid');
                longitudError.style.display = 'block';
                valido = false;
            }
            
            return valido;
        }

        // ========== FUNCIONES PARA EL CAMPO DE POWERPOINT ==========
        function inicializarPowerPointUpload() {
            const fileInput = document.getElementById('presentacionPowerPoint');
            const btnSeleccionar = document.getElementById('btnSeleccionarPowerPoint');
            const btnRemover = document.getElementById('btnRemoverPowerPoint');
            const uploadContainer = document.getElementById('powerpointUploadContainer');
            const fileInfo = document.getElementById('powerpointFileInfo');
            const fileName = document.getElementById('powerpointFileName');
            const fileSize = document.getElementById('powerpointFileSize');
            
            // Evento para el botón de seleccionar archivo
            btnSeleccionar.addEventListener('click', function() {
                fileInput.click();
            });
            
            // Evento cuando se selecciona un archivo
            fileInput.addEventListener('change', function(e) {
                if (this.files && this.files[0]) {
                    const archivo = this.files[0];
                    
                    // Validar tipo de archivo
                    const tiposPermitidos = ['.ppt', '.pptx', 'application/vnd.ms-powerpoint', 'application/vnd.openxmlformats-officedocument.presentationml.presentation'];
                    const tipoArchivo = archivo.type;
                    const extensionArchivo = archivo.name.toLowerCase().substring(archivo.name.lastIndexOf('.'));
                    
                    if (!tiposPermitidos.includes(tipoArchivo) && !tiposPermitidos.includes(extensionArchivo)) {
                        mostrarNotificacion('❌ Error: Solo se permiten archivos PowerPoint (.ppt, .pptx)', 'error', 5000);
                        this.value = '';
                        return;
                    }
                    
                    // Validar tamaño (50MB máximo)
                    const tamanoMaximo = 50 * 1024 * 1024; // 50MB en bytes
                    if (archivo.size > tamanoMaximo) {
                        mostrarNotificacion('❌ Error: El archivo es demasiado grande. Tamaño máximo: 50MB', 'error', 5000);
                        this.value = '';
                        return;
                    }
                    
                    // Mostrar información del archivo
                    fileName.textContent = archivo.name;
                    fileSize.textContent = `(${formatearTamañoArchivo(archivo.size)})`;
                    fileInfo.classList.add('show');
                    
                    // Marcar como válido
                    uploadContainer.classList.add('auto-filled');
                    
                    // Validar el campo
                    validarPowerPoint();
                    
                    mostrarNotificacion('✅ Archivo PowerPoint cargado correctamente', 'success', 3000);
                    actualizarBarraProgreso();
                }
            });
            
            // Evento para el botón de remover archivo
            btnRemover.addEventListener('click', function() {
                fileInput.value = '';
                fileInfo.classList.remove('show');
                uploadContainer.classList.remove('auto-filled');
                fileName.textContent = 'No se ha seleccionado ningún archivo';
                fileSize.textContent = '';
                
                // Validar el campo después de remover
                validarPowerPoint();
                
                mostrarNotificacion('Archivo PowerPoint removido', 'info', 3000);
                actualizarBarraProgreso();
            });
            
            // Funcionalidad de drag and drop
            uploadContainer.addEventListener('dragover', function(e) {
                e.preventDefault();
                this.classList.add('dragover');
            });
            
            uploadContainer.addEventListener('dragleave', function(e) {
                e.preventDefault();
                this.classList.remove('dragover');
            });
            
            uploadContainer.addEventListener('drop', function(e) {
                e.preventDefault();
                this.classList.remove('dragover');
                
                if (e.dataTransfer.files && e.dataTransfer.files[0]) {
                    fileInput.files = e.dataTransfer.files;
                    const event = new Event('change', { bubbles: true });
                    fileInput.dispatchEvent(event);
                }
            });
        }
        
        function formatearTamañoArchivo(bytes) {
            if (bytes === 0) return '0 Bytes';
            const k = 1024;
            const sizes = ['Bytes', 'KB', 'MB', 'GB'];
            const i = Math.floor(Math.log(bytes) / Math.log(k));
            return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
        }

        // ========== VALIDACIÓN DEL CAMPO POWERPOINT ==========
        function validarPowerPoint() {
            const fileInput = document.getElementById('presentacionPowerPoint');
            const errorElement = document.getElementById('powerpointError');
            const uploadContainer = document.getElementById('powerpointUploadContainer');
            
            if (fileInput.files.length === 0) {
                uploadContainer.classList.remove('valid');
                uploadContainer.classList.add('invalid');
                errorElement.style.display = 'block';
                return false;
            } else {
                uploadContainer.classList.remove('invalid');
                uploadContainer.classList.add('valid');
                errorElement.style.display = 'none';
                return true;
            }
        }

        function actualizarBarraProgreso() {
            const camposRequeridos = document.querySelectorAll('input[required], select[required], textarea[required]');
            const camposCompletados = Array.from(camposRequeridos).filter(campo => 
                campo.value.trim() !== '' && campo.validity.valid
            ).length;
            
            const variableSeleccionada = document.getElementById('variablesProductividad').value;
            let validacionesValidas = true;
            
            if (variableSeleccionada) {
                validacionesValidas = ejecutarValidacionesVariables(variableSeleccionada);
            }
            
            const progreso = (camposCompletados / camposRequeridos.length) * 100;
            document.getElementById('progressBar').style.width = `${progreso}%`;
            
            // Cambiar color según estado de validaciones
            if (progreso === 100 && validacionesValidas) {
                document.getElementById('progressBar').style.background = 'linear-gradient(135deg, #28a745, #20c997)';
            } else if (progreso === 100 && !validacionesValidas) {
                document.getElementById('progressBar').style.background = 'linear-gradient(135deg, #dc3545, #c82333)';
            } else {
                document.getElementById('progressBar').style.background = 'linear-gradient(135deg, #2a5298, #667eea)';
            }
        }

        function limpiarFormulario() {
            document.getElementById('operativeForm').reset();
            document.getElementById('variableSectionsContainer').innerHTML = '';
            document.getElementById('validationSummary').style.display = 'none';
            document.getElementById('progressBar').style.width = '0%';
            document.getElementById('progressBar').style.background = 'linear-gradient(135deg, #2a5298, #667eea)';
            
            // Limpiar campos de solo lectura
            document.querySelectorAll('.readonly').forEach(campo => {
                campo.value = '';
                campo.classList.remove('auto-filled');
            });
            
            // Limpiar estados
            document.getElementById('unidadesStatus').textContent = '';
            document.getElementById('dataStatus').textContent = '';
            document.getElementById('cedulaStatus').textContent = '';
            document.getElementById('locationStatus').textContent = '';
            
            // Limpiar detenidos
            document.getElementById('detenidosContainer').innerHTML = '';
            detenidoCount = 0;
            actualizarContadorDetenidos();
            
            // Ocultar sección de detenidos
            document.getElementById('seccionDetenidos').style.display = 'none';
            
            // Limpiar archivo PowerPoint
            document.getElementById('presentacionPowerPoint').value = '';
            document.getElementById('powerpointFileInfo').classList.remove('show');
            document.getElementById('powerpointUploadContainer').classList.remove('auto-filled');
            document.getElementById('powerpointFileName').textContent = 'No se ha seleccionado ningún archivo';
            document.getElementById('powerpointFileSize').textContent = '';
            
            // Validar PowerPoint después de limpiar
            validarPowerPoint();
            
            mostrarNotificacion('Formulario limpiado correctamente', 'info', 3000);
        }

        // ========== MODIFICACIÓN: FUNCIÓN ACTUALIZADA PARA GESTIÓN DE INDICADORES ADICIONALES ==========
        function gestionarIndicadoresAdicionales() {
            const nivelIndicadores = document.getElementById('nivelIndicadores').value;
            const seccionIndicadoresAdicionales = document.getElementById('indicadoresAdicionales');
            const seccionResponsable = document.getElementById('seccionResponsable');
            const seccionVariablesProductividad = document.getElementById('seccionVariablesProductividad');
            
            // Ocultar todas las secciones primero
            seccionIndicadoresAdicionales.classList.remove('active');
            seccionResponsable.style.display = 'none';
            
            if (nivelIndicadores === 'SI') {
                // Mostrar indicadores adicionales
                seccionIndicadoresAdicionales.classList.add('active');
                
                // MODIFICACIÓN: Hacer scroll a la sección de Variables de Productividad OBLIGATORIAMENTE
                setTimeout(() => {
                    seccionVariablesProductividad.scrollIntoView({ 
                        behavior: 'smooth', 
                        block: 'start' 
                    });
                    
                    // Resaltar la sección para mayor visibilidad
                    seccionVariablesProductividad.style.border = '3px solid #28a745';
                    seccionVariablesProductividad.style.boxShadow = '0 0 15px rgba(40, 167, 69, 0.5)';
                    
                    // Quitar el resaltado después de 3 segundos
                    setTimeout(() => {
                        seccionVariablesProductividad.style.border = '';
                        seccionVariablesProductividad.style.boxShadow = '';
                    }, 3000);
                    
                }, 300);
                
                // Mostrar notificación informativa
                mostrarNotificacion('Se han habilitado los indicadores adicionales. Por favor, complete la sección de Variables de Productividad.', 'info', 5000);
                
            } else if (nivelIndicadores === 'NO') {
                // Ocultar indicadores adicionales y mostrar responsable
                seccionIndicadoresAdicionales.classList.remove('active');
                seccionResponsable.style.display = 'block';
                
                // Hacer scroll a la sección del responsable
                setTimeout(() => {
                    seccionResponsable.scrollIntoView({ behavior: 'smooth', block: 'start' });
                }, 300);
                
                mostrarNotificacion('Se ha habilitado la sección del responsable', 'info', 3000);
            } else {
                // Si no se selecciona nada, mantener ocultas ambas secciones
                seccionIndicadoresAdicionales.classList.remove('active');
                seccionResponsable.style.display = 'none';
            }
            
            actualizarBarraProgreso();
        }

        // ========== EVENT LISTENERS ==========
        document.getElementById('direccionInterventora').addEventListener('change', actualizarUnidades);
        document.getElementById('numeroCedula').addEventListener('input', validarCedula);
        
        document.getElementById('presuntoDelito').addEventListener('input', function(e) {
            buscarDelitos(e.target.value);
        });
        
        document.getElementById('presuntoDelito').addEventListener('blur', function() {
            setTimeout(() => {
                document.getElementById('delitoSearchResults').style.display = 'none';
            }, 200);
        });
        
        document.getElementById('codigoSubcircuito').addEventListener('input', function(e) {
            e.target.value = e.target.value.toUpperCase();
            
            const codigo = e.target.value.trim();
            if (codigo.length >= 2) {
                clearTimeout(window.subcircuitoTimeout);
                window.subcircuitoTimeout = setTimeout(() => buscarSubcircuito(codigo), 500);
            }
            
            actualizarBarraProgreso();
        });
        
        document.getElementById('btnObtenerUbicacion').addEventListener('click', obtenerUbicacion);
        document.getElementById('btnMostrarMapa').addEventListener('click', mostrarMapa);
        document.getElementById('btnOcultarMapa').addEventListener('click', ocultarMapa);
        
        document.getElementById('latitud').addEventListener('input', function() {
            this.value = this.value.replace('.', ',');
            validarCoordenadas();
            actualizarBarraProgreso();
        });
        
        document.getElementById('longitud').addEventListener('input', function() {
            this.value = this.value.replace('.', ',');
            validarCoordenadas();
            actualizarBarraProgreso();
        });

        // Event listeners para el mapa
        document.getElementById('mapSearch').addEventListener('input', function(e) {
            buscarDireccion(e.target.value);
        });
        
        document.getElementById('mapSearch').addEventListener('focus', function() {
            const results = document.getElementById('mapSearchResults');
            if (results.children.length > 0) {
                results.style.display = 'block';
            }
        });

        // Event listener para variables de productividad
        document.getElementById('variablesProductividad').addEventListener('change', function() {
            gestionarVariableProductividad();
            actualizarBarraProgreso();
            
            // Si se cambia a una variable diferente a "Aprehendidos/Detenidos", ocultar sección de detenidos
            if (this.value !== "Aprehendidos/Detenidos") {
                document.getElementById('seccionDetenidos').style.display = 'none';
            }
        });

        // MODIFICACIÓN: Se elimina el event listener para el botón "Agregar Detenido"
        // document.getElementById('btnAgregarDetenido').addEventListener('click', function() {
        //     agregarDetenido();
        // });
        
        document.getElementById('detenidosContainer').addEventListener('click', function(e) {
            if (e.target.classList.contains('remove-detenido')) {
                const id = e.target.getAttribute('data-id');
                eliminarDetenido(id);
            }
        });

        // Event listener para indicadores adicionales
        document.getElementById('nivelIndicadores').addEventListener('change', function() {
            gestionarIndicadoresAdicionales();
            actualizarBarraProgreso();
        });

        // MODIFICACIÓN: Se elimina el event listener para el botón "Limpiar Formulario"
        // document.getElementById('btnLimpiar').addEventListener('click', limpiarFormulario);

        document.querySelectorAll('input, select, textarea').forEach(campo => {
            campo.addEventListener('input', actualizarBarraProgreso);
            campo.addEventListener('change', actualizarBarraProgreso);
        });

        document.getElementById('operativeForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            // Validar PowerPoint primero (campo obligatorio)
            if (!validarPowerPoint()) {
                mostrarNotificacion('Error: Debe adjuntar la presentación del caso en formato PowerPoint', 'error', 5000);
                // Hacer scroll al campo de PowerPoint
                document.getElementById('powerpointUploadContainer').scrollIntoView({ behavior: 'smooth', block: 'center' });
                return;
            }
            
            // Validar variables de productividad
            if (!validarTodasVariablesProductividad()) {
                mostrarNotificacion('Error: Complete correctamente el campo de la variable de productividad seleccionada', 'error', 5000);
                return;
            }
            
            // Validar detenidos
            if (!validarTodosDetenidos()) {
                mostrarNotificacion('Error: Complete correctamente los datos de los detenidos', 'error', 5000);
                return;
            }
            
            mostrarNotificacion('Formulario completo enviado correctamente', 'success', 5000);
            
            // Simular envío de datos
            console.log('Datos del operativo a enviar:');
            const datosOperativo = {
                fecha: document.getElementById('fechaOperativo').value,
                hora: document.getElementById('horaOperativo').value,
                direccionInterventora: document.getElementById('direccionInterventora').value,
                unidadInterventora: document.getElementById('unidadInterventora').value,
                presuntoDelito: document.getElementById('presuntoDelito').value,
                extractoCaso: document.getElementById('extractoCaso').value,
                coordenadas: {
                    latitud: document.getElementById('latitud').value,
                    longitud: document.getElementById('longitud').value
                },
                responsable: {
                    cedula: document.getElementById('numeroCedula').value,
                    nombres: document.getElementById('apellidosNombres').value,
                    grado: document.getElementById('grado').value
                },
                powerpoint: document.getElementById('presentacionPowerPoint').files[0] ? {
                    nombre: document.getElementById('presentacionPowerPoint').files[0].name,
                    tamaño: document.getElementById('presentacionPowerPoint').files[0].size
                } : null
            };
            
            console.log('Datos del operativo:', datosOperativo);
            
            // Datos de detenidos
            const datosDetenidos = [];
            for (let i = 1; i <= detenidoCount; i++) {
                const detenidoDiv = document.getElementById(`detenido-${i}`);
                if (!detenidoDiv) continue;
                
                const datos = {
                    nombres: document.getElementById(`nombresDetenido-${i}`)?.value || '',
                    cedula: document.getElementById(`cedulaDetenido-${i}`)?.value || '',
                    edad: document.getElementById(`edadDetenido-${i}`)?.value || '',
                    nacionalidad: document.getElementById(`nacionalidadDetenido-${i}`)?.value || '',
                    detenciones: document.getElementById(`cantidadDetenciones-${i}`)?.value || '0',
                    resolucion: document.getElementById(`resolucionFlagrancia-${i}`)?.value || '',
                    grupoDelictivo: document.getElementById(`grupoDelictivo-${i}`)?.value || '',
                    servidorPolicial: document.getElementById(`servidorPolicial-${i}`)?.value || ''
                };
                
                datosDetenidos.push(datos);
                console.log(`Detenido ${i}:`, datos);
            }
            
            console.log('Total de detenidos:', datosDetenidos.length);
            
            // Datos del archivo PowerPoint
            const archivoPowerPoint = document.getElementById('presentacionPowerPoint').files[0];
            if (archivoPowerPoint) {
                console.log('Archivo PowerPoint:', {
                    nombre: archivoPowerPoint.name,
                    tamaño: archivoPowerPoint.size,
                    tipo: archivoPowerPoint.type
                });
            }
            
            // Aquí normalmente enviarías los datos a tu servidor
            // fetch('/api/operativo', { method: 'POST', body: JSON.stringify({operativo: datosOperativo, detenidos: datosDetenidos}) })
        });

        // Cerrar sugerencias de delitos al hacer clic fuera
        document.addEventListener('click', function(e) {
            if (!e.target.closest('.delito-search-container')) {
                document.getElementById('delitoSearchResults').style.display = 'none';
            }
            if (!e.target.closest('.map-search-container')) {
                document.getElementById('mapSearchResults').style.display = 'none';
            }
        });

        // ========== INICIALIZACIÓN ==========
        document.addEventListener('DOMContentLoaded', function() {
            const ahora = new Date();
            document.getElementById('fechaOperativo').value = ahora.toISOString().split('T')[0];
            document.getElementById('horaOperativo').value = ahora.toTimeString().slice(0,5);
            
            actualizarUnidades();
            
            // Ocultar la sección de responsable inicialmente
            document.getElementById('seccionResponsable').style.display = 'none';
            
            // Inicializar el campo de PowerPoint
            inicializarPowerPointUpload();
            
            console.log('✅ Formulario completo inicializado correctamente');
            console.log('🔗 URL Cédulas:', APPS_SCRIPT_URL_CEDULA);
            console.log('🔗 URL Subcircuitos:', APPS_SCRIPT_URL_SUBCIRCUITO);
            console.log('👤 Sistema de detenidos integrado');
            console.log('🗺️ Mapa listo para usar');
            console.log('📊 Campo de PowerPoint agregado y OBLIGATORIO');
            console.log('📝 Para probar:');
            console.log('   - Cédulas: 0401400783');
            console.log('   - Subcircuitos: 13D08C05S01');
            console.log('   - Todas las 72 variables de productividad disponibles');
            console.log('   - Sección de Indicadores Adicionales agregada');
            console.log('   - Sistema AUTOMÁTICO de registro de detenidos según suma de aprehendidos + detenidos');
            console.log('🚫 BOTONES ELIMINADOS: Agregar Detenido y Limpiar Formulario');
            console.log('🔄 REDIRECCIÓN: Al seleccionar SI en indicadores, se redirige a Variables de Productividad');
            
            mostrarNotificacion('Formulario completo cargado correctamente. Seleccione una Variable de Productividad para continuar.', 'info', 4000);
        });
    </script>
</body>
</html>
