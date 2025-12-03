<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PDF Schedule Tracker</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link rel="stylesheet" href="style.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
</head>
<body>
    <div class="container">
        <!-- Header -->
        <header>
            <h1><i class="fas fa-calendar-alt"></i> PDF Schedule Tracker</h1>
            <p class="subtitle">Upload your monthly schedule PDF and track daily tasks</p>
        </header>

        <div class="main-content">
            <!-- Left Panel: Upload & Controls -->
            <div class="left-panel">
                <div class="upload-section card">
                    <h3><i class="fas fa-file-upload"></i> Upload Schedule PDF</h3>
                    <div class="upload-area" id="dropArea">
                        <i class="fas fa-cloud-upload-alt fa-3x"></i>
                        <p>Drag & drop your PDF here</p>
                        <p class="upload-hint">or click to browse</p>
                        <input type="file" id="pdfInput" accept=".pdf" hidden>
                        <button class="btn" onclick="document.getElementById('pdfInput').click()">
                            <i class="fas fa-folder-open"></i> Choose PDF
                        </button>
                    </div>
                    
                    <div class="manual-input">
                        <h4><i class="fas fa-keyboard"></i> Or Enter Manually</h4>
                        <textarea id="manualInput" placeholder="Enter schedule in format:
Day 1: Meeting at 10 AM, Report due
Day 2: Client call, Submit proposal
..."></textarea>
                        <button class="btn" onclick="processManualInput()">
                            <i class="fas fa-sync-alt"></i> Process Text
                        </button>
                    </div>
                </div>

                <div class="controls-section card">
                    <h3><i class="fas fa-sliders-h"></i> Controls</h3>
                    <div class="control-group">
                        <label for="monthSelect">Month:</label>
                        <select id="monthSelect">
                            <option value="0">January</option>
                            <option value="1">February</option>
                            <!-- All months... -->
                        </select>
                        
                        <label for="yearSelect">Year:</label>
                        <input type="number" id="yearSelect" value="2024" min="2020" max="2030">
                    </div>
                    
                    <div class="btn-group">
                        <button class="btn" onclick="clearAll()">
                            <i class="fas fa-trash"></i> Clear All
                        </button>
                        <button class="btn export-btn" onclick="exportToExcel()">
                            <i class="fas fa-file-excel"></i> Export to Excel
                        </button>
                        <button class="btn" onclick="printSchedule()">
                            <i class="fas fa-print"></i> Print
                        </button>
                    </div>
                </div>
                
                <div class="today-section card">
                    <h3><i class="fas fa-star"></i> Today's Focus</h3>
                    <div id="todayTasks">
                        <p class="no-tasks">Upload PDF to see today's tasks</p>
                    </div>
                </div>
            </div>

            <!-- Right Panel: Schedule Display -->
            <div class="right-panel">
                <div class="schedule-display card">
                    <div class="display-header">
                        <h3><i class="fas fa-table"></i> Monthly Schedule</h3>
                        <div class="view-controls">
                            <button class="view-btn active" onclick="switchView('table')">
                                <i class="fas fa-table"></i> Table
                            </button>
                            <button class="view-btn" onclick="switchView('calendar')">
                                <i class="fas fa-calendar"></i> Calendar
                            </button>
                            <button class="view-btn" onclick="switchView('list')">
                                <i class="fas fa-list"></i> List
                            </button>
                        </div>
                    </div>
                    
                    <!-- Table View -->
                    <div id="tableView" class="view-container active">
                        <div class="table-responsive">
                            <table id="scheduleTable">
                                <thead>
                                    <tr>
                                        <th>Day</th>
                                        <th>Date</th>
                                        <th>Tasks</th>
                                        <th>Status</th>
                                        <th>Priority</th>
                                        <th>Notes</th>
                                        <th>Actions</th>
                                    </tr>
                                </thead>
                                <tbody id="tableBody">
                                    <!-- Table rows will be inserted here -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                    
                    <!-- Calendar View -->
                    <div id="calendarView" class="view-container">
                        <div class="calendar-header">
                            <button class="nav-btn" onclick="changeMonth(-1)"><i class="fas fa-chevron-left"></i></button>
                            <h4 id="currentMonth">December 2024</h4>
                            <button class="nav-btn" onclick="changeMonth(1)"><i class="fas fa-chevron-right"></i></button>
                        </div>
                        <div class="calendar-grid" id="calendarGrid">
                            <!-- Calendar will be generated here -->
                        </div>
                    </div>
                    
                    <!-- List View -->
                    <div id="listView" class="view-container">
                        <div class="list-container" id="listContainer">
                            <!-- List will be generated here -->
                        </div>
                    </div>
                </div>
                
                <!-- Statistics -->
                <div class="stats-section card">
                    <h3><i class="fas fa-chart-bar"></i> Statistics</h3>
                    <div class="stats-grid">
                        <div class="stat-card">
                            <div class="stat-number" id="totalTasks">0</div>
                            <div class="stat-label">Total Tasks</div>
                        </div>
                        <div class="stat-card">
                            <div class="stat-number" id="completedTasks">0</div>
                            <div class="stat-label">Completed</div>
                        </div>
                        <div class="stat-card">
                            <div class="stat-number" id="pendingTasks">0</div>
                            <div class="stat-label">Pending</div>
                        </div>
                        <div class="stat-card">
                            <div class="stat-number" id="upcomingTasks">0</div>
                            <div class="stat-label">Upcoming</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Footer -->
        <footer>
            <p>PDF Schedule Tracker | Data is stored locally in your browser</p>
        </footer>
    </div>

    <!-- Notification -->
    <div id="notification" class="notification"></div>

    <script src="script.js"></script>
</body>
</html>
