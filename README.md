<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Professional Inbox Checker - Live Dashboard</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        :root {
            --primary: #2c3e50;
            --secondary: #3498db;
            --success: #27ae60;
            --danger: #e74c3c;
            --warning: #f39c12;
            --dark: #2c3e50;
            --light: #ecf0f1;
        }

        body {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        .header {
            background: white;
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            margin-bottom: 30px;
            text-align: center;
        }

        .header h1 {
            color: var(--primary);
            font-size: 36px;
            margin-bottom: 10px;
        }

        .header p {
            color: #666;
            font-size: 18px;
        }

        .stats-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 25px;
            margin-bottom: 30px;
        }

        .stat-card {
            background: white;
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            text-align: center;
            transition: transform 0.3s;
        }

        .stat-card:hover {
            transform: translateY(-10px);
        }

        .stat-card .icon {
            font-size: 48px;
            margin-bottom: 20px;
        }

        .stat-card .number {
            font-size: 48px;
            font-weight: bold;
            margin-bottom: 10px;
        }

        .stat-card .label {
            font-size: 18px;
            color: #666;
        }

        .progress-container {
            background: white;
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            margin-bottom: 30px;
        }

        .progress-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .progress-bar {
            height: 25px;
            background: #eee;
            border-radius: 15px;
            overflow: hidden;
            margin: 20px 0;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #27ae60, #2ecc71);
            width: 0%;
            transition: width 1.5s ease-in-out;
            position: relative;
        }

        .progress-text {
            position: absolute;
            right: 10px;
            top: 50%;
            transform: translateY(-50%);
            color: white;
            font-weight: bold;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.3);
        }

        .emails-container {
            background: white;
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            margin-bottom: 30px;
        }

        .emails-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }

        .email-card {
            background: #f8f9fa;
            border-radius: 15px;
            padding: 20px;
            border-left: 5px solid var(--secondary);
            transition: all 0.3s;
        }

        .email-card:hover {
            transform: translateX(10px);
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }

        .email-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }

        .email-address {
            font-weight: bold;
            color: var(--dark);
        }

        .email-status {
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: bold;
        }

        .status-inbox {
            background: #d4edda;
            color: #155724;
        }

        .status-spam {
            background: #f8d7da;
            color: #721c24;
        }

        .status-checking {
            background: #fff3cd;
            color: #856404;
        }

        .controls {
            display: flex;
            gap: 15px;
            flex-wrap: wrap;
            margin-top: 30px;
        }

        .btn {
            padding: 15px 30px;
            border: none;
            border-radius: 10px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .btn-primary {
            background: var(--secondary);
            color: white;
        }

        .btn-primary:hover {
            background: #2980b9;
            transform: translateY(-3px);
        }

        .btn-success {
            background: var(--success);
            color: white;
        }

        .btn-danger {
            background: var(--danger);
            color: white;
        }

        .btn-warning {
            background: var(--warning);
            color: white;
        }

        .last-update {
            margin-top: 20px;
            color: #666;
            font-style: italic;
        }

        .legend {
            display: flex;
            gap: 20px;
            margin-top: 20px;
            flex-wrap: wrap;
        }

        .legend-item {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .legend-color {
            width: 20px;
            height: 20px;
            border-radius: 5px;
        }

        .color-inbox { background: #27ae60; }
        .color-spam { background: #e74c3c; }
        .color-checking { background: #f39c12; }

        @media (max-width: 768px) {
            .header h1 {
                font-size: 28px;
            }
            
            .stat-card .number {
                font-size: 36px;
            }
            
            .emails-grid {
                grid-template-columns: 1fr;
            }
        }

        .refresh-btn {
            background: none;
            border: 2px solid var(--secondary);
            color: var(--secondary);
            padding: 10px 20px;
            border-radius: 10px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s;
        }

        .refresh-btn:hover {
            background: var(--secondary);
            color: white;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1><i class="fas fa-inbox"></i> Professional Inbox Checker</h1>
            <p>Real-time Email Delivery Monitoring Dashboard</p>
        </div>

        <div class="stats-container">
            <div class="stat-card" style="border-top: 5px solid #27ae60;">
                <div class="icon" style="color: #27ae60;">
                    <i class="fas fa-envelope-open"></i>
                </div>
                <div class="number" id="inboxCount">0</div>
                <div class="label">Inbox Delivery</div>
            </div>

            <div class="stat-card" style="border-top: 5px solid #e74c3c;">
                <div class="icon" style="color: #e74c3c;">
                    <i class="fas fa-ban"></i>
                </div>
                <div class="number" id="spamCount">0</div>
                <div class="label">Spam Detected</div>
            </div>

            <div class="stat-card" style="border-top: 5px solid #3498db;">
                <div class="icon" style="color: #3498db;">
                    <i class="fas fa-percentage"></i>
                </div>
                <div class="number" id="inboxRate">0%</div>
                <div class="label">Inbox Rate</div>
            </div>
        </div>

        <div class="progress-container">
            <div class="progress-header">
                <h2><i class="fas fa-chart-line"></i> Overall Inbox Rate Progress</h2>
                <button class="refresh-btn" onclick="refreshData()">
                    <i class="fas fa-sync-alt"></i> Refresh
                </button>
            </div>
            
            <div class="progress-bar">
                <div class="progress-fill" id="progressFill">
                    <div class="progress-text" id="progressText">0%</div>
                </div>
            </div>
            
            <div class="legend">
                <div class="legend-item">
                    <div class="legend-color color-inbox"></div>
                    <span>Inbox (Primary)</span>
                </div>
                <div class="legend-item">
                    <div class="legend-color color-spam"></div>
                    <span>Spam/Junk</span>
                </div>
                <div class="legend-item">
                    <div class="legend-color color-checking"></div>
                    <span>Checking</span>
                </div>
            </div>
            
            <div class="last-update" id="lastUpdate">
                Last updated: Just now
            </div>
        </div>

        <div class="emails-container">
            <h2><i class="fas fa-list"></i> Email Accounts Status</h2>
            <p>Monitoring your email delivery performance in real-time</p>
            
            <div class="emails-grid" id="emailsGrid">
                <!-- Email cards will be dynamically inserted here -->
            </div>
        </div>

        <div class="controls">
            <button class="btn btn-primary" onclick="startChecking()">
                <i class="fas fa-play"></i> Start Checking
            </button>
            <button class="btn btn-warning" onclick="pauseChecking()">
                <i class="fas fa-pause"></i> Pause
            </button>
            <button class="btn btn-danger" onclick="resetAll()">
                <i class="fas fa-redo"></i> Reset All
            </button>
            <button class="btn btn-success" onclick="exportReport()">
                <i class="fas fa-download"></i> Export Report
            </button>
        </div>
    </div>

    <script>
        // Email list from your request
        const emailAccounts = [
            "durjoy_barai@yahoo.com",
            "barai.durjoy@yahoo.com", 
            "durjoybadolkoushik@yahoo.com",
            "durjoy2025@yahoo.com",
            "badol.vai@aol.com",
            "mailb89@aol.com",
            "kbs.test@aol.com",
            "kbstest@yahoo.com",
            "bm2test@aol.com",
            "testmail27@aol.com",
            "koushik97099@yahoo.com",
            "koushik498@aol.com",
            "koushikmondal919@yahoo.com"
        ];

        let checkingInterval;
        let isChecking = false;

        // Initialize email data
        const emailData = emailAccounts.map(email => ({
            email: email,
            status: 'checking', // checking, inbox, spam
            lastChecked: null,
            provider: email.includes('@yahoo.com') ? 'Yahoo' : email.includes('@aol.com') ? 'AOL' : 'Unknown'
        }));

        // Initialize dashboard
        function initDashboard() {
            updateStats();
            renderEmailCards();
            updateProgressBar();
        }

        // Render email cards
        function renderEmailCards() {
            const emailsGrid = document.getElementById('emailsGrid');
            emailsGrid.innerHTML = '';
            
            emailData.forEach((item, index) => {
                const card = document.createElement('div');
                card.className = 'email-card';
                
                let statusClass = '';
                let statusText = '';
                let statusIcon = '';
                
                switch(item.status) {
                    case 'inbox':
                        statusClass = 'status-inbox';
                        statusText = 'INBOX';
                        statusIcon = 'fas fa-check-circle';
                        break;
                    case 'spam':
                        statusClass = 'status-spam';
                        statusText = 'SPAM';
                        statusIcon = 'fas fa-exclamation-triangle';
                        break;
                    default:
                        statusClass = 'status-checking';
                        statusText = 'CHECKING';
                        statusIcon = 'fas fa-sync-alt fa-spin';
                }
                
                const lastChecked = item.lastChecked 
                    ? new Date(item.lastChecked).toLocaleTimeString() 
                    : 'Not checked yet';
                
                card.innerHTML = `
                    <div class="email-header">
                        <div class="email-address">${item.email}</div>
                        <div class="email-status ${statusClass}">
                            <i class="${statusIcon}"></i> ${statusText}
                        </div>
                    </div>
                    <div style="margin-top: 10px;">
                        <div><strong>Provider:</strong> ${item.provider}</div>
                        <div><strong>Last Checked:</strong> ${lastChecked}</div>
                    </div>
                    <div style="margin-top: 15px; display: flex; gap: 10px;">
                        <button onclick="manualCheck(${index})" style="padding: 5px 10px; background: #3498db; color: white; border: none; border-radius: 5px; cursor: pointer;">
                            <i class="fas fa-sync"></i> Check Now
                        </button>
                        <button onclick="viewDetails('${item.email}')" style="padding: 5px 10px; background: #2c3e50; color: white; border: none; border-radius: 5px; cursor: pointer;">
                            <i class="fas fa-info-circle"></i> Details
                        </button>
                    </div>
                `;
                
                emailsGrid.appendChild(card);
            });
        }

        // Update statistics
        function updateStats() {
            const inboxCount = emailData.filter(item => item.status === 'inbox').length;
            const spamCount = emailData.filter(item => item.status === 'spam').length;
            const totalChecked = inboxCount + spamCount;
            const inboxRate = totalChecked > 0 ? Math.round((inboxCount / totalChecked) * 100) : 0;
            
            document.getElementById('inboxCount').textContent = inboxCount;
            document.getElementById('spamCount').textContent = spamCount;
            document.getElementById('inboxRate').textContent = `${inboxRate}%`;
            
            updateProgressBar();
            
            // Update last update time
            document.getElementById('lastUpdate').textContent = 
                `Last updated: ${new Date().toLocaleTimeString()}`;
        }

        // Update progress bar
        function updateProgressBar() {
            const inboxCount = emailData.filter(item => item.status === 'inbox').length;
            const totalChecked = emailData.filter(item => item.status !== 'checking').length;
            const inboxRate = totalChecked > 0 ? Math.round((inboxCount / totalChecked) * 100) : 0;
            
            const progressFill = document.getElementById('progressFill');
            const progressText = document.getElementById('progressText');
            
            progressFill.style.width = `${inboxRate}%`;
            progressText.textContent = `${inboxRate}%`;
            
            // Change color based on rate
            if (inboxRate >= 80) {
                progressFill.style.background = 'linear-gradient(90deg, #27ae60, #2ecc71)';
            } else if (inboxRate >= 50) {
                progressFill.style.background = 'linear-gradient(90deg, #f39c12, #f1c40f)';
            } else {
                progressFill.style.background = 'linear-gradient(90deg, #e74c3c, #c0392b)';
            }
        }

        // Simulate email checking (in real app, this would connect to email APIs)
        function simulateEmailCheck() {
            emailData.forEach((item, index) => {
                // Randomly update status for simulation
                if (Math.random() < 0.3) { // 30% chance to change status
                    const statuses = ['inbox', 'spam', 'checking'];
                    // Weighted random - 70% inbox, 20% spam, 10% checking
                    const random = Math.random();
                    let newStatus;
                    
                    if (random < 0.7) {
                        newStatus = 'inbox';
                    } else if (random < 0.9) {
                        newStatus = 'spam';
                    } else {
                        newStatus = 'checking';
                    }
                    
                    // Only update if status changed
                    if (newStatus !== item.status) {
                        emailData[index].status = newStatus;
                        emailData[index].lastChecked = new Date().toISOString();
                        
                        // Add some visual feedback
                        const emailCard = document.querySelectorAll('.email-card')[index];
                        if (emailCard) {
                            emailCard.style.animation = 'none';
                            setTimeout(() => {
                                emailCard.style.animation = 'pulse 0.5s';
                            }, 10);
                        }
                    }
                }
            });
            
            updateStats();
            renderEmailCards();
        }

        // Start automatic checking
        function startChecking() {
            if (isChecking) return;
            
            isChecking = true;
            checkingInterval = setInterval(simulateEmailCheck, 3000); // Check every 3 seconds
            
            document.querySelector('.btn-primary').innerHTML = '<i class="fas fa-play"></i> Checking...';
            document.querySelector('.btn-primary').style.background = '#2c3e50';
            
            // Show notification
            showNotification('Email checking started', 'success');
        }

        // Pause checking
        function pauseChecking() {
            if (!isChecking) return;
            
            isChecking = false;
            clearInterval(checkingInterval);
            
            document.querySelector('.btn-primary').innerHTML = '<i class="fas fa-play"></i> Start Checking';
            document.querySelector('.btn-primary').style.background = '';
            
            showNotification('Email checking paused', 'warning');
        }

        // Manual check for specific email
        function manualCheck(index) {
            // Simulate checking with loading state
            emailData[index].status = 'checking';
            emailData[index].lastChecked = new Date().toISOString();
            
            renderEmailCards();
            
            // Simulate API call delay
            setTimeout(() => {
                // Random result for simulation
                const random = Math.random();
                emailData[index].status = random > 0.3 ? 'inbox' : 'spam';
                emailData[index].lastChecked = new Date().toISOString();
                
                updateStats();
                renderEmailCards();
                
                showNotification(`Checked ${emailData[index].email}`, 'info');
            }, 1000);
        }

        // View details for email
        function viewDetails(email) {
            const item = emailData.find(e => e.email === email);
            if (!item) return;
            
            const detailWindow = window.open('', '_blank');
            detailWindow.document.write(`
                <!DOCTYPE html>
                <html>
                <head>
                    <title>Email Details - ${email}</title>
                    <style>
                        body { font-family: Arial, sans-serif; padding: 20px; }
                        .detail-item { margin: 10px 0; }
                        .status-badge { padding: 5px 10px; border-radius: 5px; color: white; }
                        .inbox { background: #27ae60; }
                        .spam { background: #e74c3c; }
                        .checking { background: #f39c12; }
                    </style>
                </head>
                <body>
                    <h2>Email Delivery Details</h2>
                    <div class="detail-item"><strong>Email:</strong> ${email}</div>
                    <div class="detail-item"><strong>Provider:</strong> ${item.provider}</div>
                    <div class="detail-item"><strong>Status:</strong> 
                        <span class="status-badge ${item.status}">${item.status.toUpperCase()}</span>
                    </div>
                    <div class="detail-item"><strong>Last Checked:</strong> ${item.lastChecked ? new Date(item.lastChecked).toLocaleString() : 'N/A'}</div>
                    <div class="detail-item"><strong>History:</strong> Last 24 hours - ${Math.floor(Math.random() * 100)}% deliverability rate</div>
                    <button onclick="window.print()" style="margin-top: 20px; padding: 10px 20px; background: #3498db; color: white; border: none; border-radius: 5px; cursor: pointer;">
                        Print Report
                    </button>
                </body>
                </html>
            `);
        }

        // Reset all data
        function resetAll() {
            if (confirm('Are you sure you want to reset all data? This cannot be undone.')) {
                emailData.forEach(item => {
                    item.status = 'checking';
                    item.lastChecked = null;
                });
                
                if (isChecking) {
                    pauseChecking();
                }
                
                initDashboard();
                showNotification('All data has been reset', 'info');
            }
        }

        // Export report
        function exportReport() {
            const inboxCount = emailData.filter(item => item.status === 'inbox').length;
            const spamCount = emailData.filter(item => item.status === 'spam').length;
            const totalChecked = inboxCount + spamCount;
            const inboxRate = totalChecked > 0 ? Math.round((inboxCount / totalChecked) * 100) : 0;
            
            const report = {
                generatedAt: new Date().toISOString(),
                summary: {
                    totalEmails: emailData.length,
                    inboxCount: inboxCount,
                    spamCount: spamCount,
                    inboxRate: inboxRate,
                    notChecked: emailData.length - totalChecked
                },
                emails: emailData.map(item => ({
                    email: item.email,
                    provider: item.provider,
                    status: item.status,
                    lastChecked: item.lastChecked
                }))
            };
            
            // Create downloadable file
            const dataStr = JSON.stringify(report, null, 2);
            const dataUri = 'data:application/json;charset=utf-8,'+ encodeURIComponent(dataStr);
            
            const exportFileDefaultName = `inbox-report-${new Date().toISOString().slice(0,10)}.json`;
            
            const linkElement = document.createElement('a');
            linkElement.setAttribute('href', dataUri);
            linkElement.setAttribute('download', exportFileDefaultName);
            linkElement.click();
            
            showNotification('Report exported successfully', 'success');
        }

        // Refresh data
        function refreshData() {
            simulateEmailCheck();
            showNotification('Data refreshed', 'info');
        }

        // Show notification
        function showNotification(message, type) {
            // Remove existing notification
            const existingNotification = document.querySelector('.notification');
            if (existingNotification) {
                existingNotification.remove();
            }
            
            // Create new notification
            const notification = document.createElement('div');
            notification.className = `notification notification-${type}`;
            notification.innerHTML = `
                <div>${message}</div>
                <button onclick="this.parentElement.remove()">&times;</button>
            `;
            
            // Add styles
            notification.style.cssText = `
                position: fixed;
                top: 20px;
                right: 20px;
                background: ${type === 'success' ? '#27ae60' : type === 'error' ? '#e74c3c' : type === 'warning' ? '#f39c12' : '#3498db'};
                color: white;
                padding: 15px 20px;
                border-radius: 10px;
                box-shadow: 0 5px 15px rgba(0,0,0,0.2);
                display: flex;
                align-items: center;
                justify-content: space-between;
                min-width: 300px;
                z-index: 1000;
                animation: slideIn 0.3s ease;
            `;
            
            document.body.appendChild(notification);
            
            // Auto remove after 5 seconds
            setTimeout(() => {
                if (notification.parentElement) {
                    notification.remove();
                }
            }, 5000);
        }

        // Add CSS animation
        const style = document.createElement('style');
        style.textContent = `
            @keyframes slideIn {
                from { transform: translateX(100%); opacity: 0; }
                to { transform: translateX(0); opacity: 1; }
            }
            
            @keyframes pulse {
                0% { transform: scale(1); }
                50% { transform: scale(1.02); }
                100% { transform: scale(1); }
            }
            
            .email-card {
                animation: none;
            }
        `;
        document.head.appendChild(style);

        // Initialize when page loads
        window.onload = function() {
            initDashboard();
            
            // Simulate some initial data
            setTimeout(() => {
                // Set some initial statuses for demo
                emailData.forEach((item, index) => {
                    if (index < 4) {
                        item.status = 'inbox';
                    } else if (index < 7) {
                        item.status = 'spam';
                    }
                    item.lastChecked = new Date().toISOString();
                });
                updateStats();
                renderEmailCards();
            }, 1000);
        };
    </script>
</body>
</html>
