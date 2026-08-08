# daily-goals
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <meta name="theme-color" content="#0f172a">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <meta name="apple-mobile-web-app-title" content="Daily Goals">
  <title>Daily Performance Tracker</title>
  <link rel="manifest" href="data:application/json;base64,eyJuYW1lIjoiRGFpbHkgUGVyZm9ybWFuY2UgVHJhY2tlciIsInNob3J0X25hbWUiOiJEYWlseSBHb2FscyIsInN0YXJ0X3VybCI6Ii4iLCJkaXNwbGF5Ijoic3RhbmRhbG9uZSIsImJhY2tncm91bmRfY29sb3IiOiIjMGYxNzJhIiwidGhlbWVfY29sb3IiOiIjMGYxNzJhIn0=">
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
      background: #0f172a;
      color: #e2e8f0;
      min-height: 100vh;
      padding-bottom: 40px;
    }
    .container { max-width: 720px; margin: 0 auto; padding: 20px 16px; }
    .header { text-align: center; margin-bottom: 24px; }
    .header h1 { font-size: 26px; font-weight: 800; color: #f8fafc; letter-spacing: -0.5px; }
    .header .date { color: #94a3b8; margin-top: 4px; font-size: 13px; font-weight: 500; }

    .progress-ring-wrap { display: flex; justify-content: center; margin-bottom: 24px; }
    .progress-ring-wrap .ring-box { position: relative; width: 150px; height: 150px; }
    .progress-ring-wrap .ring-box svg { transform: rotate(-90deg); }
    .progress-ring-wrap .ring-box .ring-text {
      position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);
      text-align: center;
    }
    .progress-ring-wrap .ring-box .ring-text .pct { font-size: 30px; font-weight: 800; color: #10b981; transition: color 0.3s; }
    .progress-ring-wrap .ring-box .ring-text .label { font-size: 10px; color: #64748b; font-weight: 600; text-transform: uppercase; letter-spacing: 1px; }

    .stats { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-bottom: 20px; }
    .stats .stat-card {
      background: #1e293b; border-radius: 12px; padding: 14px 8px; text-align: center;
      border: 1px solid #334155;
    }
    .stats .stat-card .num { font-size: 22px; font-weight: 700; }
    .stats .stat-card .num.total { color: #38bdf8; }
    .stats .stat-card .num.done { color: #10b981; }
    .stats .stat-card .num.streak { color: #f59e0b; }
    .stats .stat-card .lbl { font-size: 10px; color: #64748b; margin-top: 2px; font-weight: 500; }

    .add-row { display: flex; gap: 10px; margin-bottom: 16px; }
    .add-row input {
      flex: 1; background: #1e293b; border: 1px solid #334155; border-radius: 10px;
      padding: 12px 14px; color: #f1f5f9; font-size: 15px; outline: none;
    }
    .add-row input:focus { border-color: #38bdf8; }
    .add-row button {
      background: #38bdf8; color: #0f172a; border: none; border-radius: 10px;
      padding: 12px 18px; font-weight: 700; font-size: 14px; cursor: pointer;
    }
    .add-row button:active { transform: scale(0.96); }

    .filters { display: flex; gap: 8px; margin-bottom: 14px; flex-wrap: wrap; }
    .filters button {
      background: #1e293b; color: #94a3b8; border: 1px solid #334155; border-radius: 20px;
      padding: 6px 14px; font-size: 12px; font-weight: 600; cursor: pointer; transition: all 0.2s;
    }
    .filters button.active { background: #38bdf8; color: #0f172a; border-color: #38bdf8; }
    .filters button:active { transform: scale(0.95); }

    .goals-list { display: flex; flex-direction: column; gap: 10px; }
    .goal-item {
      display: flex; align-items: center; gap: 12px;
      background: #1e293b; border-radius: 12px; padding: 14px 14px;
      border: 1px solid #334155; transition: all 0.2s;
    }
    .goal-item.done { opacity: 0.65; border-color: #10b981; }
    .goal-item .check-btn {
      width: 24px; height: 24px; border-radius: 6px; border: 2px solid #334155;
      background: transparent; display: flex; align-items: center; justify-content: center;
      cursor: pointer; font-size: 14px; font-weight: 700; flex-shrink: 0; transition: all 0.2s;
      color: transparent;
    }
    .goal-item .check-btn.checked { background: #10b981; border-color: #10b981; color: #0f172a; }
    .goal-item .goal-info { flex: 1; min-width: 0; }
    .goal-item .goal-info .text {
      font-size: 15px; font-weight: 500; color: #f1f5f9; word-break: break-word;
      transition: all 0.2s;
    }
    .goal-item.done .goal-info .text { color: #64748b; text-decoration: line-through; }
    .goal-item .goal-info .cat {
      font-size: 11px; margin-top: 3px; font-weight: 600;
    }
    .goal-item .del-btn {
      background: transparent; border: none; color: #475569; cursor: pointer;
      font-size: 18px; padding: 4px; transition: color 0.2s;
    }
    .goal-item .del-btn:active { color: #ef4444; }

    .empty-state { text-align: center; padding: 40px 20px; color: #64748b; }
    .empty-state .emoji { font-size: 40px; margin-bottom: 8px; }
    .empty-state .txt { font-size: 14px; font-weight: 500; }

    .week-box {
      margin-top: 24px; background: #1e293b; border-radius: 14px;
      padding: 16px; border: 1px solid #334155;
    }
    .week-box .title {
      font-size: 12px; font-weight: 700; color: #94a3b8; margin-bottom: 10px;
      text-transform: uppercase; letter-spacing: 1px;
    }
    .week-grid { display: grid; grid-template-columns: repeat(7, 1fr); gap: 8px; }
    .week-grid .day-cell {
      text-align: center; padding: 10px 2px; border-radius: 10px; transition: all 0.2s;
    }
    .week-grid .day-cell .dname { font-size: 10px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px; }
    .week-grid .day-cell .dnum { font-size: 16px; font-weight: 700; margin-top: 2px; }
    .week-grid .day-cell.today { background: #38bdf8; }
    .week-grid .day-cell.today .dname, .week-grid .day-cell.today .dnum { color: #0f172a; }
    .week-grid .day-cell.past-good { background: #10b981; }
    .week-grid .day-cell.past-good .dname, .week-grid .day-cell.past-good .dnum { color: #0f172a; }
    .week-grid .day-cell.past-bad { background: #ef4444; }
    .week-grid .day-cell.past-bad .dname, .week-grid .day-cell.past-bad .dnum { color: #0f172a; }
    .week-grid .day-cell.future { background: #1e293b; border: 1px solid #334155; }
    .week-grid .day-cell.future .dname, .week-grid .day-cell.future .dnum { color: #64748b; }

    .reset-wrap { text-align: center; margin-top: 18px; }
    .reset-wrap button {
      background: transparent; color: #64748b; border: 1px solid #334155;
      border-radius: 8px; padding: 8px 16px; font-size: 12px; cursor: pointer;
    }
    .reset-wrap button:active { color: #ef4444; border-color: #ef4444; }

    .toast {
      position: fixed; bottom: 24px; left: 50%; transform: translateX(-50%) translateY(100px);
      background: #1e293b; color: #f1f5f9; padding: 12px 24px; border-radius: 10px;
      border: 1px solid #334155; font-size: 13px; font-weight: 500;
      opacity: 0; transition: all 0.3s ease; z-index: 100; white-space: nowrap;
    }
    .toast.show { transform: translateX(-50%) translateY(0); opacity: 1; }
  </style>
<base target="_blank">
</head>
<body>
  <div class="container">
    <div class="header">
      <h1>🎯 Daily Goals</h1>
      <div class="date" id="dateDisplay"></div>
    </div>

    <div class="progress-ring-wrap">
      <div class="ring-box">
        <svg width="150" height="150" viewBox="0 0 150 150">
          <circle cx="75" cy="75" r="64" fill="none" stroke="#1e293b" stroke-width="12"/>
          <circle id="progressRing" cx="75" cy="75" r="64" fill="none" stroke="#10b981" stroke-width="12" stroke-linecap="round" stroke-dasharray="402" stroke-dashoffset="402" style="transition: stroke-dashoffset 0.6s ease;"/>
        </svg>
        <div class="ring-text">
          <div class="pct" id="pctText">0%</div>
          <div class="label">Done</div>
        </div>
      </div>
    </div>

    <div class="stats">
      <div class="stat-card"><div class="num total" id="statTotal">0</div><div class="lbl">Total</div></div>
      <div class="stat-card"><div class="num done" id="statDone">0</div><div class="lbl">Completed</div></div>
      <div class="stat-card"><div class="num streak" id="statStreak">0</div><div class="lbl">Streak</div></div>
    </div>

    <div class="add-row">
      <input type="text" id="goalInput" placeholder="Add a new daily goal..." onkeydown="if(event.key==='Enter') addGoal()">
      <button onclick="addGoal()">+ Add</button>
    </div>

    <div class="filters">
      <button class="active" onclick="filterGoals('all', this)">All</button>
      <button onclick="filterGoals('health', this)">💪 Health</button>
      <button onclick="filterGoals('work', this)">💼 Work</button>
      <button onclick="filterGoals('learning', this)">📚 Learning</button>
      <button onclick="filterGoals('personal', this)">🌱 Personal</button>
    </div>

    <div class="goals-list" id="goalsList"></div>
    <div class="empty-state" id="emptyState" style="display:none;">
      <div class="emoji">📝</div>
      <div class="txt">No goals yet. Add your first one above!</div>
    </div>

    <div class="week-box">
      <div class="title">This Week</div>
      <div class="week-grid" id="weekGrid"></div>
    </div>

    <div class="reset-wrap">
      <button onclick="resetDay()">↺ Reset Day</button>
    </div>
  </div>

  <div class="toast" id="toast"></div>

  <script>
    const STORAGE_KEY = 'dgt_goals_v1';
    const STREAK_KEY = 'dgt_streak_v1';
    const LAST_DATE_KEY = 'dgt_lastdate_v1';
    const HISTORY_KEY = 'dgt_history_v1';

    let goals = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');
    let history = JSON.parse(localStorage.getItem(HISTORY_KEY) || '{}');
    let filter = 'all';
    let streak = parseInt(localStorage.getItem(STREAK_KEY) || '0');
    let lastDate = localStorage.getItem(LAST_DATE_KEY) || '';

    const today = new Date().toDateString();
    const todayKey = new Date().toISOString().split('T')[0];
    const days = ['Sun','Mon','Tue','Wed','Thu','Fri','Sat'];

    const catColors = { health: '#10b981', work: '#38bdf8', learning: '#a78bfa', personal: '#f59e0b', default: '#94a3b8' };
    const catIcons = { health: '💪', work: '💼', learning: '📚', personal: '🌱', default: '📌' };

    function detectCategory(text) {
      const t = text.toLowerCase();
      if (/gym|run|workout|exercise|walk|yoga|meditate|sleep|water|eat|diet|health|protein|cardio|steps/.test(t)) return 'health';
      if (/work|project|email|meeting|deadline|task|report|client|slack|code|deploy|design/.test(t)) return 'work';
      if (/read|book|course|learn|study|language|skill|practice|tutorial|video/.test(t)) return 'learning';
      if (/journal|gratitude|family|call|friend|hobby|clean|organize|budget|save|invest/.test(t)) return 'personal';
      return 'default';
    }

    // Handle new day rollover
    if (lastDate && lastDate !== today) {
      const yesterday = new Date(); yesterday.setDate(yesterday.getDate() - 1);
      const allDone = goals.length > 0 && goals.every(g => g.done);
      history[lastDate] = { total: goals.length, done: goals.filter(g => g.done).length };
      localStorage.setItem(HISTORY_KEY, JSON.stringify(history));
      if (allDone && lastDate === yesterday.toDateString()) streak++;
      else if (!allDone) streak = 0;
      goals = goals.map(g => ({...g, done: false}));
      localStorage.setItem(STORAGE_KEY, JSON.stringify(goals));
      localStorage.setItem(STREAK_KEY, streak.toString());
    }
    if (!lastDate) { localStorage.setItem(LAST_DATE_KEY, today); localStorage.setItem(STREAK_KEY, '0'); }
    else { localStorage.setItem(LAST_DATE_KEY, today); }

    document.getElementById('dateDisplay').textContent = new Date().toLocaleDateString('en-US', { weekday: 'long', month: 'short', day: 'numeric' });

    function save() { localStorage.setItem(STORAGE_KEY, JSON.stringify(goals)); render(); }

    function showToast(msg) {
      const t = document.getElementById('toast');
      t.textContent = msg; t.classList.add('show');
      setTimeout(() => t.classList.remove('show'), 2000);
    }

    function updateProgress() {
      const total = goals.length;
      const done = goals.filter(g => g.done).length;
      const pct = total === 0 ? 0 : Math.round((done / total) * 100);
      const circumference = 2 * Math.PI * 64;
      const offset = circumference - (pct / 100) * circumference;
      document.getElementById('progressRing').style.strokeDashoffset = offset;
      document.getElementById('pctText').textContent = pct + '%';
      document.getElementById('statTotal').textContent = total;
      document.getElementById('statDone').textContent = done;
      document.getElementById('statStreak').textContent = streak;

      const ring = document.getElementById('progressRing');
      const pctText = document.getElementById('pctText');
      if (pct < 30) { ring.style.stroke = '#ef4444'; pctText.style.color = '#ef4444'; }
      else if (pct < 70) { ring.style.stroke = '#f59e0b'; pctText.style.color = '#f59e0b'; }
      else { ring.style.stroke = '#10b981'; pctText.style.color = '#10b981'; }
    }

    function addGoal() {
      const input = document.getElementById('goalInput');
      const text = input.value.trim();
      if (!text) return;
      const cat = detectCategory(text);
      goals.push({ id: Date.now(), text, done: false, category: cat });
      input.value = '';
      save();
      showToast('Goal added!');
    }

    function toggleGoal(id) {
      const g = goals.find(x => x.id === id);
      if (g) { g.done = !g.done; save(); if (g.done) showToast('Nice! Keep going!'); }
    }

    function deleteGoal(id) {
      goals = goals.filter(x => x.id !== id);
      save();
      showToast('Goal removed');
    }

    function filterGoals(f, btn) {
      filter = f;
      document.querySelectorAll('.filters button').forEach(b => b.classList.remove('active'));
      if (btn) btn.classList.add('active');
      render();
    }

    function resetDay() {
      if (confirm('Reset all goals for today?')) {
        goals = goals.map(g => ({...g, done: false}));
        save();
        showToast('Day reset');
      }
    }

    function render() {
      const list = document.getElementById('goalsList');
      const empty = document.getElementById('emptyState');
      const filtered = filter === 'all' ? goals : goals.filter(g => g.category === filter || (filter === 'personal' && g.category === 'default'));

      if (goals.length === 0) {
        list.style.display = 'none'; empty.style.display = 'block';
      } else {
        list.style.display = 'flex'; empty.style.display = 'none';
        list.innerHTML = filtered.map(g => {
          const color = catColors[g.category] || catColors.default;
          const icon = catIcons[g.category] || catIcons.default;
          return `
            <div class="goal-item ${g.done ? 'done' : ''}">
              <button class="check-btn ${g.done ? 'checked' : ''}" onclick="toggleGoal(${g.id})" style="border-color: ${g.done ? '#10b981' : color};">
                ${g.done ? '✓' : ''}
              </button>
              <div class="goal-info">
                <div class="text">${escapeHtml(g.text)}</div>
                <div class="cat" style="color: ${color};">${icon} ${g.category === 'default' ? 'personal' : g.category}</div>
              </div>
              <button class="del-btn" onclick="deleteGoal(${g.id})">🗑</button>
            </div>
          `;
        }).join('');
      }
      updateProgress();
      renderWeek();
    }

    function escapeHtml(text) {
      const div = document.createElement('div'); div.textContent = text; return div.innerHTML;
    }

    function renderWeek() {
      const grid = document.getElementById('weekGrid');
      const now = new Date();
      const dayOfWeek = now.getDay();
      const startOfWeek = new Date(now);
      startOfWeek.setDate(now.getDate() - dayOfWeek);

      let html = '';
      for (let i = 0; i < 7; i++) {
        const d = new Date(startOfWeek);
        d.setDate(startOfWeek.getDate() + i);
        const dStr = d.toDateString();
        const dKey = d.toISOString().split('T')[0];
        const isToday = dStr === today;
        const isPast = d < new Date(today);
        const isFuture = d > new Date(today);

        let cls = '';
        if (isToday) cls = 'today';
        else if (isFuture) cls = 'future';
        else if (history[dStr]) {
          const h = history[dStr];
          cls = h.total > 0 && h.done === h.total ? 'past-good' : 'past-bad';
        } else {
          cls = 'past-bad';
        }

        html += `<div class="day-cell ${cls}"><div class="dname">${days[d.getDay()]}</div><div class="dnum">${d.getDate()}</div></div>`;
      }
      grid.innerHTML = html;
    }

    // Save today's progress on visibility change
    document.addEventListener('visibilitychange', () => {
      if (document.hidden) {
        const done = goals.filter(g => g.done).length;
        history[today] = { total: goals.length, done };
        localStorage.setItem(HISTORY_KEY, JSON.stringify(history));
      }
    });

    render();
  </script>
</body>
</html>
