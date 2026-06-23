<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>NEXUS ZONE | SEZ Business Registry & Control Platform</title>
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@400;600;700&family=JetBrains+Mono:wght@400;600&display=swap"
      rel="stylesheet"
    />
    <style>
      :root {
        --bg: #0a0f1e;
        --bg2: #070a14;
        --panel: rgba(20, 36, 68, 0.55);
        --panel2: rgba(22, 42, 84, 0.35);
        --stroke: rgba(201, 168, 76, 0.25);
        --stroke2: rgba(120, 150, 255, 0.25);
        --text: #eaf0ff;
        --muted: rgba(234, 240, 255, 0.7);
        --muted2: rgba(234, 240, 255, 0.55);
        --gold: #c9a84c;
        --slate: #1e2a45;
        --blue: #4da3ff;
        --green: #2fe38b;
        --amber: #f2b84b;
        --red: #e35d6a;
        --ink: #071022;

        --shadowGold: 0 0 0 1px rgba(201, 168, 76, 0.22), 0 10px 50px rgba(0, 0, 0, 0.55);
        --shadowPanel: 0 0 0 1px rgba(120, 150, 255, 0.12), 0 16px 60px rgba(0, 0, 0, 0.5);

        --radius-xl: 18px;
        --radius-lg: 14px;
        --radius-md: 12px;
        --radius-sm: 10px;
        --radius-xs: 9px;

        --mono: "JetBrains Mono", ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas,
          "Liberation Mono", "Courier New", monospace;
        --serif: "Cormorant Garamond", ui-serif, Georgia, "Times New Roman", Times, serif;

        --sidebarW: 280px;
        --sidebarWCollapsed: 82px;
      }

      body.light {
        --bg: #f4f7ff;
        --bg2: #eef2ff;
        --panel: rgba(255, 255, 255, 0.62);
        --panel2: rgba(255, 255, 255, 0.4);
        --stroke: rgba(201, 168, 76, 0.35);
        --stroke2: rgba(60, 110, 255, 0.25);
        --text: #091025;
        --muted: rgba(9, 16, 37, 0.68);
        --muted2: rgba(9, 16, 37, 0.52);
        --shadowGold: 0 0 0 1px rgba(201, 168, 76, 0.28), 0 10px 50px rgba(0, 0, 0, 0.12);
        --shadowPanel: 0 0 0 1px rgba(60, 110, 255, 0.13), 0 16px 60px rgba(0, 0, 0, 0.12);
      }

      * {
        box-sizing: border-box;
      }
      html,
      body {
        height: 100%;
      }
      body {
        margin: 0;
        font-family: var(--mono);
        background: radial-gradient(1100px 700px at 10% 0%, rgba(201, 168, 76, 0.08), transparent 55%),
          radial-gradient(900px 500px at 95% 10%, rgba(77, 163, 255, 0.10), transparent 50%),
          linear-gradient(180deg, var(--bg), var(--bg2));
        color: var(--text);
        overflow-x: hidden;
      }

      /* Animated dot grid backdrop */
      .bg-grid {
        position: fixed;
        inset: 0;
        pointer-events: none;
        z-index: 0;
        background-image: radial-gradient(circle at 1px 1px, rgba(201, 168, 76, 0.16) 1px, transparent 1px);
        background-size: 22px 22px;
        opacity: 0.55;
        animation: gridPulse 9s ease-in-out infinite;
        transform: translateZ(0);
      }
      body.light .bg-grid {
        opacity: 0.35;
      }

      @keyframes gridPulse {
        0% {
          filter: hue-rotate(0deg);
          transform: scale(1) translate3d(0, 0, 0);
        }
        50% {
          filter: hue-rotate(8deg);
          transform: scale(1.02) translate3d(0, 0, 0);
        }
        100% {
          filter: hue-rotate(0deg);
          transform: scale(1) translate3d(0, 0, 0);
        }
      }

      /* Layout */
      .app {
        position: relative;
        z-index: 1;
        display: grid;
        grid-template-columns: var(--sidebarW) 1fr;
        min-height: 100vh;
      }

      @media (max-width: 980px) {
        .app {
          grid-template-columns: 1fr;
        }
      }

      .sidebar {
        position: sticky;
        top: 0;
        height: 100vh;
        padding: 18px 14px;
        border-right: 1px solid rgba(201, 168, 76, 0.12);
        background: linear-gradient(180deg, rgba(10, 15, 30, 0.85), rgba(10, 15, 30, 0.35));
        backdrop-filter: blur(10px);
        transition: width 240ms ease, transform 240ms ease, padding 240ms ease;
        overflow: hidden;
      }
      @media (max-width: 980px) {
        .sidebar {
          position: fixed;
          inset: 0 auto 0 0;
          width: min(340px, 88vw);
          transform: translateX(-110%);
          z-index: 20;
          border-right: 1px solid rgba(201, 168, 76, 0.18);
        }
        .sidebar.open {
          transform: translateX(0%);
        }
      }

      .sidebar.collapsed {
        width: var(--sidebarWCollapsed);
        padding-left: 10px;
        padding-right: 10px;
      }

      .sidebar-top {
        display: flex;
        align-items: center;
        gap: 12px;
        padding: 8px 6px 14px 6px;
      }

      .logo {
        display: flex;
        align-items: center;
        gap: 10px;
        min-width: 0;
      }
      .logo .mark {
        width: 36px;
        height: 36px;
        display: grid;
        place-items: center;
        border-radius: 12px;
        background: radial-gradient(circle at 30% 25%, rgba(201, 168, 76, 0.9), rgba(201, 168, 76, 0.08)),
          linear-gradient(180deg, rgba(201, 168, 76, 0.16), transparent);
        box-shadow: 0 0 0 1px rgba(201, 168, 76, 0.25), 0 12px 30px rgba(0, 0, 0, 0.35);
      }
      .logo h1 {
        margin: 0;
        font-family: var(--serif);
        letter-spacing: 0.4px;
        font-size: 20px;
        line-height: 1.05;
        white-space: nowrap;
      }
      .logo .sub {
        font-size: 12px;
        color: var(--muted2);
        margin-top: 2px;
      }
      .sidebar.collapsed .logo .text {
        display: none;
      }

      .role-badge {
        margin-top: 8px;
        display: flex;
        align-items: center;
        gap: 8px;
        padding: 8px 10px;
        border-radius: 999px;
        background: rgba(201, 168, 76, 0.10);
        border: 1px solid rgba(201, 168, 76, 0.25);
        box-shadow: 0 0 20px rgba(201, 168, 76, 0.10);
        color: var(--gold);
        font-size: 12px;
        font-weight: 700;
        letter-spacing: 0.3px;
      }

      .nav {
        margin-top: 16px;
        display: flex;
        flex-direction: column;
        gap: 8px;
      }

      .nav-btn {
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: flex-start;
        gap: 12px;
        padding: 12px 12px;
        border-radius: 14px;
        background: transparent;
        border: 1px solid transparent;
        color: var(--muted);
        cursor: pointer;
        transition: background 180ms ease, border-color 180ms ease, transform 180ms ease, color 180ms ease;
        user-select: none;
      }
      .nav-btn:hover {
        transform: translateY(-1px);
        background: rgba(30, 42, 69, 0.35);
        border-color: rgba(120, 150, 255, 0.18);
        color: var(--text);
      }
      .nav-btn.active {
        background: linear-gradient(180deg, rgba(201, 168, 76, 0.16), rgba(30, 42, 69, 0.28));
        border-color: rgba(201, 168, 76, 0.32);
        color: var(--text);
        box-shadow: 0 0 0 1px rgba(201, 168, 76, 0.12), 0 0 35px rgba(201, 168, 76, 0.12);
      }
      .nav-btn .icon {
        width: 28px;
        text-align: center;
        font-size: 16px;
      }
      .sidebar.collapsed .nav-btn .label {
        display: none;
      }

      /* Main */
      .main {
        padding: 20px 22px 46px 22px;
      }
      @media (max-width: 980px) {
        .main {
          padding: 16px 14px 46px 14px;
        }
      }

      .mobile-topbar {
        display: none;
      }
      @media (max-width: 980px) {
        .mobile-topbar {
          display: flex;
          align-items: center;
          justify-content: space-between;
          gap: 12px;
          margin-bottom: 14px;
        }
      }

      .hamburger {
        border: 1px solid rgba(201, 168, 76, 0.25);
        background: rgba(201, 168, 76, 0.08);
        color: var(--gold);
        border-radius: 12px;
        padding: 10px 12px;
        cursor: pointer;
        transition: transform 160ms ease, background 160ms ease;
      }
      .hamburger:hover {
        transform: translateY(-1px);
        background: rgba(201, 168, 76, 0.12);
      }

      .topbar {
        position: sticky;
        top: 0;
        z-index: 5;
        padding: 12px 14px;
        border-radius: var(--radius-lg);
        background: linear-gradient(180deg, rgba(10, 15, 30, 0.65), rgba(10, 15, 30, 0.35));
        border: 1px solid rgba(201, 168, 76, 0.14);
        box-shadow: 0 0 0 1px rgba(120, 150, 255, 0.08);
        backdrop-filter: blur(14px);
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 14px;
        margin-bottom: 18px;
      }

      .topbar-left {
        display: flex;
        align-items: center;
        gap: 12px;
        min-width: 0;
      }
      .welcome {
        display: flex;
        flex-direction: column;
        min-width: 0;
      }
      .welcome .title {
        font-family: var(--serif);
        font-size: 20px;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
      }
      .welcome .date {
        font-size: 12px;
        color: var(--muted2);
        margin-top: 2px;
      }

      .topbar-mid {
        flex: 1;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 10px;
        min-width: 240px;
      }
      .search {
        width: min(680px, 100%);
        position: relative;
      }
      .search input {
        width: 100%;
        padding: 12px 14px 12px 44px;
        border-radius: 14px;
        background: rgba(15, 22, 45, 0.55);
        border: 1px solid rgba(120, 150, 255, 0.18);
        color: var(--text);
        outline: none;
        transition: border-color 160ms ease, box-shadow 160ms ease;
      }
      body.light .search input {
        background: rgba(255, 255, 255, 0.72);
        border-color: rgba(60, 110, 255, 0.20);
      }
      .search input:focus {
        border-color: rgba(201, 168, 76, 0.45);
        box-shadow: 0 0 0 3px rgba(201, 168, 76, 0.16);
      }
      .search .lens {
        position: absolute;
        left: 14px;
        top: 50%;
        transform: translateY(-50%);
        opacity: 0.9;
        color: var(--gold);
      }
      .search-results {
        position: absolute;
        left: 0;
        right: 0;
        top: calc(100% + 8px);
        background: rgba(10, 15, 30, 0.85);
        border: 1px solid rgba(201, 168, 76, 0.22);
        border-radius: 14px;
        box-shadow: var(--shadowPanel);
        overflow: hidden;
        display: none;
        max-height: 320px;
        overflow-y: auto;
      }
      body.light .search-results {
        background: rgba(255, 255, 255, 0.92);
      }
      .search-results.show {
        display: block;
      }
      .search-results .item {
        padding: 12px 12px;
        display: flex;
        gap: 10px;
        align-items: center;
        cursor: pointer;
        border-top: 1px solid rgba(201, 168, 76, 0.08);
      }
      .search-results .item:first-child {
        border-top: none;
      }
      .search-results .item:hover {
        background: rgba(201, 168, 76, 0.10);
      }
      .search-results .pill {
        font-size: 12px;
        padding: 6px 10px;
        border-radius: 999px;
        background: rgba(30, 42, 69, 0.55);
        border: 1px solid rgba(120, 150, 255, 0.18);
        color: var(--muted);
        white-space: nowrap;
      }
      .search-results .txt {
        min-width: 0;
      }
      .search-results .txt .a {
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
      }
      .search-results .txt .b {
        font-size: 12px;
        color: var(--muted2);
        margin-top: 2px;
      }

      .topbar-right {
        display: flex;
        align-items: center;
        justify-content: flex-end;
        gap: 10px;
      }

      .icon-btn {
        width: 42px;
        height: 42px;
        border-radius: 14px;
        border: 1px solid rgba(120, 150, 255, 0.18);
        background: rgba(15, 22, 45, 0.45);
        color: var(--text);
        cursor: pointer;
        transition: transform 160ms ease, border-color 160ms ease, background 160ms ease;
        display: grid;
        place-items: center;
      }
      body.light .icon-btn {
        background: rgba(255, 255, 255, 0.7);
      }
      .icon-btn:hover {
        transform: translateY(-1px);
        border-color: rgba(201, 168, 76, 0.35);
        background: rgba(201, 168, 76, 0.08);
      }

      .notif-wrap {
        position: relative;
      }
      .notif-panel {
        position: absolute;
        top: calc(100% + 12px);
        right: 0;
        width: min(420px, 86vw);
        background: rgba(10, 15, 30, 0.9);
        border: 1px solid rgba(201, 168, 76, 0.22);
        border-radius: 16px;
        box-shadow: var(--shadowPanel);
        overflow: hidden;
        display: none;
      }
      body.light .notif-panel {
        background: rgba(255, 255, 255, 0.93);
      }
      .notif-panel.show {
        display: block;
      }
      .notif-panel .head {
        padding: 12px 14px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        border-bottom: 1px solid rgba(201, 168, 76, 0.10);
      }
      .notif-panel .head .h {
        font-family: var(--serif);
        font-size: 18px;
      }
      .notif-panel .list {
        max-height: 320px;
        overflow-y: auto;
      }
      .notif-item {
        padding: 12px 14px;
        display: flex;
        gap: 12px;
        border-top: 1px solid rgba(201, 168, 76, 0.08);
        cursor: pointer;
      }
      .notif-item:hover {
        background: rgba(201, 168, 76, 0.08);
      }
      .notif-dot {
        width: 12px;
        height: 12px;
        border-radius: 99px;
        margin-top: 3px;
        box-shadow: 0 0 24px rgba(0, 0, 0, 0.1);
      }
      .notif-item .txt {
        min-width: 0;
      }
      .notif-item .txt .a {
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
      }
      .notif-item .txt .b {
        margin-top: 3px;
        font-size: 12px;
        color: var(--muted2);
      }

      .avatar {
        width: 42px;
        height: 42px;
        border-radius: 16px;
        border: 1px solid rgba(201, 168, 76, 0.25);
        background: radial-gradient(circle at 30% 25%, rgba(201, 168, 76, 0.35), rgba(77, 163, 255, 0.12)),
          rgba(15, 22, 45, 0.45);
        display: grid;
        place-items: center;
        font-family: var(--serif);
        font-weight: 700;
      }

      /* Floating GOD MODE badge */
      .god-float {
        position: fixed;
        top: 18px;
        right: 18px;
        z-index: 50;
        padding: 10px 14px;
        border-radius: 999px;
        background: rgba(201, 168, 76, 0.14);
        border: 1px solid rgba(201, 168, 76, 0.35);
        color: var(--gold);
        box-shadow: 0 0 40px rgba(201, 168, 76, 0.18), 0 0 0 1px rgba(201, 168, 76, 0.12);
        cursor: pointer;
        user-select: none;
        display: flex;
        align-items: center;
        gap: 10px;
        transition: transform 160ms ease;
      }
      .god-float:hover {
        transform: translateY(-2px);
      }
      .god-float .bolt {
        width: 26px;
        height: 26px;
        border-radius: 10px;
        display: grid;
        place-items: center;
        background: rgba(201, 168, 76, 0.12);
        border: 1px solid rgba(201, 168, 76, 0.25);
      }
      @media (max-width: 980px) {
        .god-float {
          right: 12px;
          top: 12px;
          padding: 9px 12px;
        }
      }

      /* Views */
      .view {
        display: none;
      }
      .view.active {
        display: block;
      }

      /* Animation on load */
      .fade-stagger {
        opacity: 0;
        transform: translateY(10px);
        transition: opacity 600ms ease, transform 600ms ease;
      }
      .fade-stagger.visible {
        opacity: 1;
        transform: translateY(0);
      }

      .section-title {
        font-family: var(--serif);
        font-size: 26px;
        margin: 4px 0 14px 0;
        letter-spacing: 0.4px;
      }
      .subtext {
        color: var(--muted2);
        font-size: 12px;
        margin-top: -10px;
        margin-bottom: 18px;
      }

      /* Cards */
      .kpi-row {
        display: grid;
        grid-template-columns: repeat(6, minmax(140px, 1fr));
        gap: 14px;
        margin-bottom: 18px;
      }
      @media (max-width: 1180px) {
        .kpi-row {
          grid-template-columns: repeat(3, minmax(140px, 1fr));
        }
      }
      @media (max-width: 560px) {
        .kpi-row {
          grid-template-columns: repeat(2, minmax(120px, 1fr));
        }
      }

      .card {
        border-radius: var(--radius-xl);
        background: linear-gradient(180deg, rgba(30, 42, 69, 0.48), rgba(15, 22, 45, 0.32));
        border: 1px solid rgba(201, 168, 76, 0.18);
        box-shadow: var(--shadowPanel);
        padding: 14px 14px;
        transition: transform 180ms ease, border-color 180ms ease, box-shadow 180ms ease;
        position: relative;
        overflow: hidden;
      }
      .card:before {
        content: "";
        position: absolute;
        inset: -40px;
        background: radial-gradient(circle at 30% 20%, rgba(201, 168, 76, 0.22), transparent 55%);
        opacity: 0.0;
        transition: opacity 180ms ease;
      }
      .card:hover {
        transform: translateY(-3px);
        border-color: rgba(201, 168, 76, 0.32);
        box-shadow: var(--shadowGold);
      }
      .card:hover:before {
        opacity: 1;
      }
      .kpi-card .label {
        color: var(--muted2);
        font-size: 12px;
        margin-bottom: 10px;
        letter-spacing: 0.2px;
      }
      .kpi-card .value {
        font-family: var(--serif);
        font-size: 30px;
        font-weight: 700;
        letter-spacing: 0.3px;
      }
      .kpi-card .trend {
        margin-top: 10px;
        font-size: 12px;
        color: var(--muted);
        display: flex;
        align-items: center;
        gap: 8px;
      }

      /* Two column layout */
      .grid-2 {
        display: grid;
        grid-template-columns: 1.05fr 0.95fr;
        gap: 16px;
      }
      @media (max-width: 980px) {
        .grid-2 {
          grid-template-columns: 1fr;
        }
      }

      .list-card {
        padding: 14px 14px;
      }
      .card-head {
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 12px;
        margin-bottom: 12px;
      }
      .card-head .h {
        font-family: var(--serif);
        font-size: 18px;
        letter-spacing: 0.2px;
      }
      .card-head .meta {
        color: var(--muted2);
        font-size: 12px;
      }

      .feed {
        max-height: 340px;
        overflow-y: auto;
        padding-right: 6px;
      }
      .feed-item {
        display: flex;
        gap: 12px;
        padding: 12px 12px;
        border-radius: 14px;
        background: rgba(30, 42, 69, 0.20);
        border: 1px solid rgba(120, 150, 255, 0.12);
        transition: transform 180ms ease, border-color 180ms ease, background 180ms ease;
        margin-bottom: 10px;
      }
      .feed-item:hover {
        transform: translateY(-2px);
        border-color: rgba(201, 168, 76, 0.28);
        background: rgba(201, 168, 76, 0.06);
      }
      .feed-item .avatar-mini {
        width: 38px;
        height: 38px;
        border-radius: 16px;
        display: grid;
        place-items: center;
        border: 1px solid rgba(201, 168, 76, 0.20);
        background: rgba(15, 22, 45, 0.45);
        flex: none;
      }
      .feed-item .t {
        min-width: 0;
      }
      .feed-item .t .a {
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        font-size: 13px;
      }
      .feed-item .t .b {
        margin-top: 4px;
        color: var(--muted2);
        font-size: 12px;
      }

      /* Mini-table */
      .table-wrap {
        overflow: auto;
        border-radius: 16px;
        border: 1px solid rgba(120, 150, 255, 0.12);
        background: rgba(10, 15, 30, 0.25);
      }
      body.light .table-wrap {
        background: rgba(255, 255, 255, 0.4);
      }
      table {
        width: 100%;
        border-collapse: separate;
        border-spacing: 0;
        font-size: 12px;
      }
      thead th {
        position: sticky;
        top: 0;
        z-index: 2;
        background: linear-gradient(180deg, rgba(15, 22, 45, 0.92), rgba(15, 22, 45, 0.55));
        border-bottom: 1px solid rgba(201, 168, 76, 0.18);
        padding: 12px 10px;
        text-align: left;
        color: var(--muted);
        font-weight: 700;
        letter-spacing: 0.2px;
        white-space: nowrap;
      }
      body.light thead th {
        background: linear-gradient(180deg, rgba(255, 255, 255, 0.98), rgba(255, 255, 255, 0.62));
      }
      tbody td {
        padding: 12px 10px;
        border-bottom: 1px solid rgba(120, 150, 255, 0.10);
      }
      tbody tr:nth-child(even) td {
        background: rgba(30, 42, 69, 0.16);
      }
      tbody tr:hover td {
        background: rgba(201, 168, 76, 0.08);
      }

      .sortable {
        cursor: pointer;
      }
      .sortable:hover {
        color: var(--text);
      }

      /* Badges */
      .pill {
        display: inline-flex;
        align-items: center;
        gap: 8px;
        padding: 6px 10px;
        border-radius: 999px;
        border: 1px solid rgba(255, 255, 255, 0.10);
        font-size: 12px;
        color: var(--muted);
        background: rgba(30, 42, 69, 0.35);
        white-space: nowrap;
      }
      .pill:before {
        content: "";
        width: 9px;
        height: 9px;
        border-radius: 99px;
        background: rgba(255, 255, 255, 0.4);
        box-shadow: 0 0 18px rgba(0, 0, 0, 0.25);
      }

      .pill.active {
        color: rgba(47, 227, 139, 0.95);
        border-color: rgba(47, 227, 139, 0.28);
        background: rgba(47, 227, 139, 0.08);
      }
      .pill.active:before {
        background: rgba(47, 227, 139, 0.9);
      }
      .pill.pending {
        color: rgba(242, 184, 75, 0.98);
        border-color: rgba(242, 184, 75, 0.28);
        background: rgba(242, 184, 75, 0.08);
      }
      .pill.pending:before {
        background: rgba(242, 184, 75, 0.9);
      }
      .pill.suspended {
        color: rgba(227, 93, 106, 0.98);
        border-color: rgba(227, 93, 106, 0.3);
        background: rgba(227, 93, 106, 0.08);
      }
      .pill.suspended:before {
        background: rgba(227, 93, 106, 0.95);
      }
      .pill.processing {
        color: rgba(77, 163, 255, 0.98);
        border-color: rgba(77, 163, 255, 0.3);
        background: rgba(77, 163, 255, 0.08);
      }
      .pill.processing:before {
        background: rgba(77, 163, 255, 0.95);
      }
      .pill.deregistered {
        color: rgba(30, 42, 69, 0.95);
        border-color: rgba(20, 30, 55, 0.5);
        background: rgba(20, 30, 55, 0.35);
      }
      .pill.deregistered:before {
        background: rgba(20, 30, 55, 0.85);
      }

      .priority {
        display: inline-flex;
        align-items: center;
        justify-content: center;
        padding: 6px 10px;
        border-radius: 999px;
        font-size: 12px;
        font-weight: 700;
        border: 1px solid rgba(120, 150, 255, 0.18);
        background: rgba(30, 42, 69, 0.25);
        color: var(--muted);
        white-space: nowrap;
      }
      .priority.high {
        color: rgba(227, 93, 106, 1);
        border-color: rgba(227, 93, 106, 0.35);
        background: rgba(227, 93, 106, 0.08);
      }
      .priority.medium {
        color: rgba(242, 184, 75, 1);
        border-color: rgba(242, 184, 75, 0.35);
        background: rgba(242, 184, 75, 0.08);
      }
      .priority.low {
        color: rgba(77, 163, 255, 1);
        border-color: rgba(77, 163, 255, 0.35);
        background: rgba(77, 163, 255, 0.08);
      }

      /* Buttons */
      .btn {
        border-radius: 14px;
        padding: 10px 12px;
        border: 1px solid rgba(120, 150, 255, 0.18);
        background: rgba(15, 22, 45, 0.45);
        color: var(--text);
        cursor: pointer;
        transition: transform 160ms ease, border-color 160ms ease, background 160ms ease;
        font-family: var(--mono);
        font-size: 12px;
        display: inline-flex;
        align-items: center;
        gap: 10px;
        user-select: none;
      }
      body.light .btn {
        background: rgba(255, 255, 255, 0.72);
      }
      .btn:hover {
        transform: translateY(-1px);
        border-color: rgba(201, 168, 76, 0.32);
        background: rgba(201, 168, 76, 0.08);
      }
      .btn.primary {
        border-color: rgba(201, 168, 76, 0.40);
        background: rgba(201, 168, 76, 0.14);
        color: var(--gold);
      }
      .btn.danger {
        border-color: rgba(227, 93, 106, 0.40);
        background: rgba(227, 93, 106, 0.10);
        color: rgba(227, 93, 106, 1);
      }
      .btn.blue {
        border-color: rgba(77, 163, 255, 0.40);
        background: rgba(77, 163, 255, 0.10);
        color: rgba(77, 163, 255, 1);
      }
      .btn.small {
        padding: 8px 10px;
        border-radius: 12px;
        font-size: 12px;
      }

      .btn.ghost {
        background: transparent;
        border-color: rgba(201, 168, 76, 0.18);
      }

      /* Dashboard map */
      .map-card {
        padding: 14px 14px;
        margin-top: 18px;
      }
      .map-wrap {
        background: rgba(30, 42, 69, 0.18);
        border: 1px solid rgba(120, 150, 255, 0.12);
        border-radius: 18px;
        padding: 14px;
      }
      .map-meta {
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 12px;
        margin-bottom: 10px;
      }
      .legend {
        display: flex;
        gap: 10px;
        flex-wrap: wrap;
      }
      .leg {
        display: flex;
        gap: 8px;
        align-items: center;
        font-size: 12px;
        color: var(--muted2);
      }
      .swatch {
        width: 12px;
        height: 12px;
        border-radius: 3px;
        border: 1px solid rgba(255, 255, 255, 0.1);
      }
      .swatch.green {
        background: rgba(47, 227, 139, 0.25);
        border-color: rgba(47, 227, 139, 0.55);
      }
      .swatch.amber {
        background: rgba(242, 184, 75, 0.25);
        border-color: rgba(242, 184, 75, 0.55);
      }
      .swatch.red {
        background: rgba(227, 93, 106, 0.20);
        border-color: rgba(227, 93, 106, 0.55);
      }

      .map-svg {
        width: 100%;
        height: 260px;
      }
      .zone-sector {
        cursor: pointer;
        transition: filter 180ms ease, transform 180ms ease, opacity 180ms ease;
      }
      .zone-sector:hover {
        filter: brightness(1.18);
        transform: translateY(-1px);
      }

      .zone-details {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 14px;
        margin-top: 12px;
      }
      @media (max-width: 780px) {
        .zone-details {
          grid-template-columns: 1fr;
        }
      }

      .occupancy-bar {
        border-radius: 14px;
        background: rgba(30, 42, 69, 0.22);
        border: 1px solid rgba(120, 150, 255, 0.12);
        padding: 12px 12px;
      }
      .occupancy-bar .row {
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 10px;
      }
      .occupancy-bar .name {
        font-family: var(--serif);
        font-size: 18px;
      }
      .occupancy-bar .pct {
        font-size: 12px;
        color: var(--muted2);
      }
      .bar-track {
        margin-top: 10px;
        height: 12px;
        border-radius: 999px;
        background: rgba(120, 150, 255, 0.15);
        overflow: hidden;
        border: 1px solid rgba(120, 150, 255, 0.12);
      }
      .bar-fill {
        height: 100%;
        width: 50%;
        border-radius: 999px;
        background: linear-gradient(90deg, rgba(47, 227, 139, 0.35), rgba(201, 168, 76, 0.45));
        transition: width 260ms ease;
      }
      .bar-fill.amber {
        background: linear-gradient(90deg, rgba(242, 184, 75, 0.35), rgba(201, 168, 76, 0.45));
      }
      .bar-fill.red {
        background: linear-gradient(90deg, rgba(227, 93, 106, 0.35), rgba(201, 168, 76, 0.45));
      }

      .util-grid {
        border-radius: 14px;
        background: rgba(30, 42, 69, 0.22);
        border: 1px solid rgba(120, 150, 255, 0.12);
        padding: 12px 12px;
      }
      .util-grid .uhead {
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 10px;
      }
      .util-grid .uhead .name {
        font-family: var(--serif);
        font-size: 18px;
      }
      .util-grid .items {
        margin-top: 10px;
        display: flex;
        flex-wrap: wrap;
        gap: 10px;
      }
      .util-pill {
        display: flex;
        align-items: center;
        gap: 10px;
        padding: 8px 10px;
        border-radius: 999px;
        border: 1px solid rgba(120, 150, 255, 0.18);
        background: rgba(15, 22, 45, 0.35);
        font-size: 12px;
        color: var(--muted);
        white-space: nowrap;
      }
      .util-pill .s {
        width: 10px;
        height: 10px;
        border-radius: 999px;
        background: rgba(47, 227, 139, 0.7);
        box-shadow: 0 0 24px rgba(47, 227, 139, 0.15);
      }
      .util-pill.amber .s {
        background: rgba(242, 184, 75, 0.9);
        box-shadow: 0 0 24px rgba(242, 184, 75, 0.15);
      }
      .util-pill.red .s {
        background: rgba(227, 93, 106, 0.95);
        box-shadow: 0 0 24px rgba(227, 93, 106, 0.15);
      }

      /* Filters */
      .toolbar {
        display: flex;
        gap: 12px;
        align-items: flex-start;
        justify-content: space-between;
        flex-wrap: wrap;
        margin-bottom: 14px;
      }
      .filters {
        display: flex;
        gap: 10px;
        flex-wrap: wrap;
        align-items: center;
      }
      .field {
        display: flex;
        flex-direction: column;
        gap: 6px;
        min-width: 170px;
      }
      .field .lab {
        font-size: 12px;
        color: var(--muted2);
      }
      .field input,
      .field select {
        padding: 11px 12px;
        border-radius: 14px;
        border: 1px solid rgba(120, 150, 255, 0.18);
        background: rgba(15, 22, 45, 0.45);
        color: var(--text);
        outline: none;
      }
      body.light .field input,
      body.light .field select {
        background: rgba(255, 255, 255, 0.72);
      }
      .field input:focus,
      .field select:focus {
        border-color: rgba(201, 168, 76, 0.45);
        box-shadow: 0 0 0 3px rgba(201, 168, 76, 0.16);
      }

      /* Drawer */
      .drawer-backdrop {
        position: fixed;
        inset: 0;
        background: rgba(0, 0, 0, 0.45);
        backdrop-filter: blur(4px);
        display: none;
        z-index: 60;
        opacity: 0;
        transition: opacity 180ms ease;
      }
      .drawer-backdrop.show {
        display: block;
        opacity: 1;
      }
      .drawer {
        position: fixed;
        top: 0;
        right: 0;
        height: 100vh;
        width: min(560px, 96vw);
        background: linear-gradient(180deg, rgba(10, 15, 30, 0.95), rgba(10, 15, 30, 0.78));
        border-left: 1px solid rgba(201, 168, 76, 0.22);
        z-index: 70;
        transform: translateX(110%);
        transition: transform 240ms ease;
        display: flex;
        flex-direction: column;
      }
      .drawer.open {
        transform: translateX(0%);
      }
      .drawer .dhead {
        padding: 16px 16px 12px 16px;
        border-bottom: 1px solid rgba(201, 168, 76, 0.14);
        display: flex;
        align-items: flex-start;
        justify-content: space-between;
        gap: 12px;
      }
      .drawer .dhead .left {
        min-width: 0;
      }
      .drawer .dhead .name {
        font-family: var(--serif);
        font-size: 22px;
        margin-bottom: 6px;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
      }
      .drawer .dhead .meta {
        display: flex;
        gap: 10px;
        flex-wrap: wrap;
        align-items: center;
      }
      .reg-badge {
        padding: 6px 10px;
        border-radius: 999px;
        border: 1px solid rgba(201, 168, 76, 0.25);
        background: rgba(201, 168, 76, 0.08);
        color: var(--gold);
        font-size: 12px;
        font-weight: 700;
        letter-spacing: 0.2px;
      }
      .drawer .close-btn {
        width: 42px;
        height: 42px;
        border-radius: 16px;
        border: 1px solid rgba(120, 150, 255, 0.18);
        background: rgba(15, 22, 45, 0.45);
        color: var(--text);
        cursor: pointer;
      }
      .drawer .dbody {
        padding: 14px 16px 20px 16px;
        overflow-y: auto;
      }

      .tabs {
        display: flex;
        gap: 10px;
        flex-wrap: wrap;
        margin-bottom: 12px;
      }
      .tab {
        border-radius: 999px;
        padding: 8px 12px;
        border: 1px solid rgba(120, 150, 255, 0.18);
        background: rgba(15, 22, 45, 0.35);
        color: var(--muted);
        cursor: pointer;
        user-select: none;
        transition: border-color 160ms ease, background 160ms ease, transform 160ms ease;
        font-size: 12px;
        font-weight: 700;
      }
      .tab:hover {
        transform: translateY(-1px);
        border-color: rgba(201, 168, 76, 0.32);
        background: rgba(201, 168, 76, 0.08);
        color: var(--text);
      }
      .tab.active {
        border-color: rgba(201, 168, 76, 0.45);
        background: rgba(201, 168, 76, 0.14);
        color: var(--gold);
      }
      .drawer-section {
        display: none;
      }
      .drawer-section.active {
        display: block;
      }
      .kv-grid {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 12px;
      }
      @media (max-width: 500px) {
        .kv-grid {
          grid-template-columns: 1fr;
        }
      }
      .kv {
        padding: 12px 12px;
        border-radius: 16px;
        border: 1px solid rgba(120, 150, 255, 0.12);
        background: rgba(30, 42, 69, 0.18);
      }
      .kv .k {
        color: var(--muted2);
        font-size: 12px;
      }
      .kv .v {
        margin-top: 6px;
        font-size: 13px;
        color: var(--text);
      }

      .quick-actions {
        margin-top: 14px;
        padding: 12px 12px;
        border-radius: 18px;
        background: rgba(30, 42, 69, 0.16);
        border: 1px solid rgba(201, 168, 76, 0.16);
      }
      .quick-actions .qahead {
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 10px;
        margin-bottom: 10px;
      }
      .quick-actions .qahead .t {
        font-family: var(--serif);
        font-size: 18px;
      }
      .quick-actions .list {
        display: flex;
        flex-wrap: wrap;
        gap: 10px;
      }

      .doc-list {
        display: grid;
        grid-template-columns: 1fr;
        gap: 10px;
      }
      .doc-item {
        border-radius: 16px;
        border: 1px solid rgba(120, 150, 255, 0.12);
        background: rgba(30, 42, 69, 0.18);
        padding: 12px 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 12px;
      }
      .doc-item .left {
        min-width: 0;
      }
      .doc-item .left .a {
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        font-size: 13px;
      }
      .doc-item .left .b {
        margin-top: 4px;
        color: var(--muted2);
        font-size: 12px;
      }

      /* Modal / overlays */
      .modal-backdrop {
        position: fixed;
        inset: 0;
        background: rgba(0, 0, 0, 0.52);
        backdrop-filter: blur(4px);
        display: none;
        align-items: center;
        justify-content: center;
        z-index: 90;
        padding: 18px;
      }
      .modal-backdrop.show {
        display: flex;
      }

      .modal {
        width: min(520px, 96vw);
        border-radius: 22px;
        background: linear-gradient(180deg, rgba(10, 15, 30, 0.95), rgba(10, 15, 30, 0.75));
        border: 1px solid rgba(201, 168, 76, 0.24);
        box-shadow: var(--shadowPanel);
        padding: 16px 16px;
      }
      body.light .modal {
        background: linear-gradient(180deg, rgba(255, 255, 255, 0.96), rgba(255, 255, 255, 0.72));
      }
      .modal .mhead {
        display: flex;
        align-items: flex-start;
        justify-content: space-between;
        gap: 12px;
        padding-bottom: 12px;
        border-bottom: 1px solid rgba(201, 168, 76, 0.12);
      }
      .modal .mhead .h {
        font-family: var(--serif);
        font-size: 20px;
      }
      .modal .mhead .x {
        width: 42px;
        height: 42px;
        border-radius: 16px;
        border: 1px solid rgba(120, 150, 255, 0.18);
        background: rgba(15, 22, 45, 0.45);
        color: var(--text);
        cursor: pointer;
      }
      .modal .mbody {
        padding-top: 12px;
      }
      .pin-hint {
        color: var(--muted2);
        font-size: 12px;
        margin: 6px 0 12px 0;
      }
      .pin-row {
        display: flex;
        gap: 10px;
        justify-content: center;
        align-items: center;
      }
      .pin-digit {
        width: 52px;
        height: 56px;
        border-radius: 16px;
        border: 1px solid rgba(120, 150, 255, 0.18);
        background: rgba(15, 22, 45, 0.45);
        display: grid;
        place-items: center;
        font-family: var(--mono);
        font-size: 22px;
        color: var(--gold);
        box-shadow: 0 0 0 1px rgba(0, 0, 0, 0.15);
        transition: transform 160ms ease, border-color 160ms ease;
      }
      .pin-digit.active {
        transform: translateY(-2px);
        border-color: rgba(201, 168, 76, 0.45);
        box-shadow: 0 0 0 3px rgba(201, 168, 76, 0.15);
      }
      .pin-digit.error {
        border-color: rgba(227, 93, 106, 0.55);
        box-shadow: 0 0 0 3px rgba(227, 93, 106, 0.15);
        animation: shake 240ms ease-in-out;
      }
      @keyframes shake {
        0% {
          transform: translateX(0);
        }
        25% {
          transform: translateX(-6px);
        }
        50% {
          transform: translateX(6px);
        }
        75% {
          transform: translateX(-4px);
        }
        100% {
          transform: translateX(0);
        }
      }
      .pin-actions {
        display: flex;
        justify-content: flex-end;
        gap: 10px;
        margin-top: 14px;
      }
      .pin-input-hidden {
        position: absolute;
        opacity: 0;
        pointer-events: none;
      }

      /* Context menu */
      .context-menu {
        position: fixed;
        z-index: 80;
        width: 220px;
        background: rgba(10, 15, 30, 0.92);
        border: 1px solid rgba(201, 168, 76, 0.25);
        border-radius: 16px;
        box-shadow: var(--shadowPanel);
        overflow: hidden;
        display: none;
      }
      body.light .context-menu {
        background: rgba(255, 255, 255, 0.93);
      }
      .context-menu.show {
        display: block;
      }
      .context-menu .item {
        padding: 12px 12px;
        cursor: pointer;
        border-top: 1px solid rgba(201, 168, 76, 0.08);
        display: flex;
        gap: 10px;
        align-items: center;
        color: var(--muted);
        font-size: 12px;
      }
      .context-menu .item:first-child {
        border-top: none;
      }
      .context-menu .item:hover {
        background: rgba(201, 168, 76, 0.09);
        color: var(--text);
      }

      /* Toast */
      .toasts {
        position: fixed;
        bottom: 14px;
        left: 14px;
        z-index: 95;
        display: flex;
        flex-direction: column;
        gap: 10px;
      }
      .toast {
        border-radius: 16px;
        background: rgba(10, 15, 30, 0.90);
        border: 1px solid rgba(201, 168, 76, 0.25);
        box-shadow: var(--shadowPanel);
        padding: 12px 12px;
        min-width: 260px;
        animation: toastIn 240ms ease;
      }
      body.light .toast {
        background: rgba(255, 255, 255, 0.95);
      }
      @keyframes toastIn {
        from {
          opacity: 0;
          transform: translateY(8px);
        }
        to {
          opacity: 1;
          transform: translateY(0);
        }
      }
      .toast .t {
        font-family: var(--serif);
        font-size: 16px;
      }
      .toast .s {
        margin-top: 3px;
        color: var(--muted2);
        font-size: 12px;
      }

      /* Kanban */
      .kanban {
        display: grid;
        grid-template-columns: repeat(5, minmax(170px, 1fr));
        gap: 12px;
      }
      @media (max-width: 1160px) {
        .kanban {
          grid-template-columns: repeat(2, minmax(170px, 1fr));
        }
      }
      @media (max-width: 620px) {
        .kanban {
          grid-template-columns: 1fr;
        }
      }
      .kan-col {
        border-radius: 18px;
        border: 1px solid rgba(120, 150, 255, 0.14);
        background: rgba(30, 42, 69, 0.14);
        padding: 12px 12px;
        min-height: 260px;
      }
      .kan-col .hc {
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 10px;
        margin-bottom: 10px;
      }
      .kan-col .hc .t {
        font-family: var(--serif);
        font-size: 18px;
      }
      .kan-col .hc .count {
        color: var(--muted2);
        font-size: 12px;
      }
      .kan-card {
        border-radius: 16px;
        border: 1px solid rgba(201, 168, 76, 0.18);
        background: rgba(15, 22, 45, 0.38);
        padding: 12px 12px;
        margin-bottom: 10px;
        transition: transform 180ms ease, border-color 180ms ease, background 180ms ease;
      }
      .kan-card:hover {
        transform: translateY(-2px);
        border-color: rgba(201, 168, 76, 0.32);
        background: rgba(201, 168, 76, 0.07);
      }
      .kan-card .top {
        display: flex;
        align-items: flex-start;
        justify-content: space-between;
        gap: 10px;
      }
      .kan-card .cn {
        font-size: 13px;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        max-width: 100%;
      }
      .kan-card .meta {
        margin-top: 6px;
        color: var(--muted2);
        font-size: 12px;
      }
      .reviewer {
        display: inline-flex;
        align-items: center;
        gap: 8px;
      }
      .reviewer .av {
        width: 22px;
        height: 22px;
        border-radius: 10px;
        border: 1px solid rgba(201, 168, 76, 0.22);
        background: rgba(30, 42, 69, 0.35);
        display: grid;
        place-items: center;
        font-size: 11px;
        color: var(--gold);
        font-weight: 700;
        flex: none;
      }
      .progress-sla {
        margin-top: 10px;
        border-radius: 999px;
        height: 10px;
        background: rgba(120, 150, 255, 0.15);
        border: 1px solid rgba(120, 150, 255, 0.12);
        overflow: hidden;
      }
      .progress-sla .fill {
        height: 100%;
        width: 60%;
        background: rgba(47, 227, 139, 0.7);
        transition: width 260ms ease;
      }
      .progress-sla.amber .fill {
        background: rgba(242, 184, 75, 0.75);
      }
      .progress-sla.red .fill {
        background: rgba(227, 93, 106, 0.82);
      }
      .kan-card .act {
        display: flex;
        justify-content: flex-end;
        gap: 10px;
        margin-top: 12px;
      }

      /* License grid */
      .filter-cards {
        display: flex;
        flex-wrap: wrap;
        gap: 10px;
        margin-bottom: 14px;
      }
      .fcard {
        cursor: pointer;
        border-radius: 999px;
        padding: 10px 14px;
        border: 1px solid rgba(120, 150, 255, 0.18);
        background: rgba(15, 22, 45, 0.35);
        color: var(--muted);
        transition: transform 160ms ease, border-color 160ms ease, background 160ms ease, color 160ms ease;
        user-select: none;
        font-size: 12px;
        font-weight: 700;
      }
      .fcard:hover {
        transform: translateY(-1px);
        border-color: rgba(201, 168, 76, 0.32);
        background: rgba(201, 168, 76, 0.08);
        color: var(--text);
      }
      .fcard.active {
        border-color: rgba(201, 168, 76, 0.45);
        background: rgba(201, 168, 76, 0.14);
        color: var(--gold);
      }

      /* Authority override panels */
      .god-panel {
        border-radius: 22px;
        padding: 14px 14px;
        border: 1px solid rgba(201, 168, 76, 0.28);
        background: linear-gradient(180deg, rgba(201, 168, 76, 0.10), rgba(30, 42, 69, 0.14));
        box-shadow: 0 0 40px rgba(201, 168, 76, 0.10);
      }
      .god-panel .ghead {
        display: flex;
        align-items: baseline;
        justify-content: space-between;
        gap: 10px;
        margin-bottom: 10px;
      }
      .god-panel .ghead .t {
        font-family: var(--serif);
        font-size: 20px;
      }
      .god-panel .ghead .note {
        font-size: 12px;
        color: var(--muted2);
      }
      .god-panel .controls {
        display: flex;
        flex-wrap: wrap;
        gap: 10px;
      }

      .warning-badge {
        display: inline-flex;
        gap: 8px;
        align-items: center;
        padding: 6px 10px;
        border-radius: 999px;
        border: 1px solid rgba(227, 93, 106, 0.35);
        background: rgba(227, 93, 106, 0.10);
        color: rgba(227, 93, 106, 1);
        font-size: 12px;
        font-weight: 700;
      }

      /* Investor cards */
      .investor-grid {
        display: grid;
        grid-template-columns: repeat(3, minmax(220px, 1fr));
        gap: 14px;
      }
      @media (max-width: 980px) {
        .investor-grid {
          grid-template-columns: repeat(2, minmax(220px, 1fr));
        }
      }
      @media (max-width: 580px) {
        .investor-grid {
          grid-template-columns: 1fr;
        }
      }
      .profile-card {
        padding: 14px 14px;
      }
      .profile-top {
        display: flex;
        align-items: center;
        gap: 12px;
      }
      .profile-top .av {
        width: 56px;
        height: 56px;
        border-radius: 20px;
        border: 1px solid rgba(201, 168, 76, 0.22);
        background: rgba(15, 22, 45, 0.35);
        display: grid;
        place-items: center;
        font-family: var(--serif);
        font-size: 18px;
        color: var(--gold);
        font-weight: 700;
        flex: none;
      }
      .profile-top .nm {
        min-width: 0;
      }
      .profile-top .nm .a {
        font-family: var(--serif);
        font-size: 18px;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
      }
      .profile-top .nm .b {
        margin-top: 4px;
        color: var(--muted2);
        font-size: 12px;
      }

      .profile-kv {
        margin-top: 12px;
        display: grid;
        grid-template-columns: 1fr;
        gap: 10px;
      }
      .profile-kv .row {
        padding: 12px 12px;
        border-radius: 16px;
        border: 1px solid rgba(120, 150, 255, 0.12);
        background: rgba(30, 42, 69, 0.18);
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 12px;
      }
      .profile-kv .row .k {
        color: var(--muted2);
        font-size: 12px;
      }
      .profile-kv .row .v {
        font-size: 12px;
        color: var(--text);
        font-weight: 700;
        white-space: nowrap;
      }
      .profile-act {
        margin-top: 12px;
        display: flex;
        justify-content: flex-end;
      }

      /* Gauge & CSS charts */
      .gauge {
        height: 140px;
        display: grid;
        place-items: center;
      }
      .gauge .ring {
        width: 120px;
        height: 120px;
        border-radius: 999px;
        background: conic-gradient(rgba(47, 227, 139, 0.9) 0deg, rgba(47, 227, 139, 0.9) var(--deg), rgba(120, 150, 255, 0.18) var(--deg), rgba(120, 150, 255, 0.18) 360deg);
        display: grid;
        place-items: center;
        position: relative;
        box-shadow: 0 0 50px rgba(47, 227, 139, 0.12);
      }
      .gauge .ring:before {
        content: "";
        width: 88px;
        height: 88px;
        border-radius: 999px;
        background: linear-gradient(180deg, rgba(30, 42, 69, 0.55), rgba(15, 22, 45, 0.35));
        border: 1px solid rgba(120, 150, 255, 0.16);
        position: absolute;
      }
      .gauge .ring .txt {
        position: relative;
        font-family: var(--serif);
        font-size: 26px;
        font-weight: 700;
      }
      .gauge .ring .sub {
        margin-top: 4px;
        font-size: 12px;
        color: var(--muted2);
      }

      .donut {
        width: 160px;
        height: 160px;
        border-radius: 999px;
        background: conic-gradient(
          rgba(201, 168, 76, 0.95) 0 144deg,
          rgba(77, 163, 255, 0.85) 144deg 216deg,
          rgba(242, 184, 75, 0.9) 216deg 270deg,
          rgba(227, 93, 106, 0.85) 270deg 298deg,
          rgba(120, 150, 255, 0.65) 298deg 360deg
        );
        position: relative;
        display: grid;
        place-items: center;
        box-shadow: 0 0 60px rgba(201, 168, 76, 0.12);
      }
      .donut:before {
        content: "";
        position: absolute;
        width: 110px;
        height: 110px;
        border-radius: 999px;
        background: linear-gradient(180deg, rgba(30, 42, 69, 0.55), rgba(15, 22, 45, 0.35));
        border: 1px solid rgba(120, 150, 255, 0.16);
      }
      .donut .center {
        position: relative;
        text-align: center;
      }
      .donut .center .a {
        font-family: var(--serif);
        font-size: 24px;
        font-weight: 700;
      }
      .donut .center .b {
        margin-top: 4px;
        color: var(--muted2);
        font-size: 12px;
      }

      .bars {
        display: flex;
        gap: 10px;
        align-items: flex-end;
        height: 180px;
        padding: 10px 10px 0 10px;
      }
      .bars .bar {
        flex: 1;
        min-width: 14px;
        border-radius: 10px;
        background: linear-gradient(180deg, rgba(201, 168, 76, 0.75), rgba(77, 163, 255, 0.22));
        border: 1px solid rgba(201, 168, 76, 0.20);
        box-shadow: 0 0 30px rgba(201, 168, 76, 0.08);
        opacity: 0.95;
      }

      /* Risk matrix */
      .risk-matrix {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 12px;
      }
      .risk-cell {
        border-radius: 16px;
        border: 1px solid rgba(120, 150, 255, 0.12);
        background: rgba(30, 42, 69, 0.18);
        padding: 12px 12px;
        min-height: 120px;
        position: relative;
      }
      .risk-cell .lab {
        color: var(--muted2);
        font-size: 12px;
      }
      .risk-cell .dots {
        margin-top: 10px;
        display: flex;
        flex-wrap: wrap;
        gap: 8px;
      }
      .risk-dot {
        width: 14px;
        height: 14px;
        border-radius: 999px;
        border: 1px solid rgba(255, 255, 255, 0.14);
        background: rgba(47, 227, 139, 0.25);
        box-shadow: 0 0 24px rgba(47, 227, 139, 0.10);
      }
      .risk-dot.amber {
        background: rgba(242, 184, 75, 0.28);
        box-shadow: 0 0 24px rgba(242, 184, 75, 0.12);
      }
      .risk-dot.red {
        background: rgba(227, 93, 106, 0.30);
        box-shadow: 0 0 24px rgba(227, 93, 106, 0.12);
      }

      /* Accessibility helper */
      .sr-only {
        position: absolute;
        width: 1px;
        height: 1px;
        padding: 0;
        margin: -1px;
        overflow: hidden;
        clip: rect(0, 0, 0, 0);
        white-space: nowrap;
        border-width: 0;
      }

      /* Toggle switch */
      .switch {
        position: relative;
        display: inline-flex;
        align-items: center;
      }
      .switch input {
        position: absolute;
        opacity: 0;
        pointer-events: none;
      }
      .switch .track {
        width: 46px;
        height: 26px;
        border-radius: 999px;
        background: rgba(120, 150, 255, 0.12);
        border: 1px solid rgba(120, 150, 255, 0.22);
        transition: background 160ms ease, border-color 160ms ease;
        position: relative;
      }
      .switch .track::after {
        content: "";
        width: 20px;
        height: 20px;
        border-radius: 999px;
        background: rgba(201, 168, 76, 0.85);
        box-shadow: 0 0 30px rgba(201, 168, 76, 0.16);
        position: absolute;
        top: 2px;
        left: 3px;
        transition: transform 160ms ease, left 160ms ease;
      }
      .switch input:checked + .track {
        background: rgba(201, 168, 76, 0.16);
        border-color: rgba(201, 168, 76, 0.42);
      }
      .switch input:checked + .track::after {
        left: 22px;
      }

      /* Sidebar overlay for mobile */
      .sidebar-backdrop {
        position: fixed;
        inset: 0;
        background: rgba(0, 0, 0, 0.35);
        display: none;
        z-index: 10;
      }
      .sidebar-backdrop.show {
        display: block;
      }
    </style>
  </head>

  <body>
    <div class="bg-grid" aria-hidden="true"></div>
    <div id="app" class="app">
      <aside id="sidebar" class="sidebar" aria-label="Sidebar Navigation">
        <div class="sidebar-top">
          <div class="logo">
            <div class="mark" aria-hidden="true">
              <svg width="20" height="18" viewBox="0 0 20 18" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path
                  d="M10 0.8L18.2 5.6V12.4L10 17.2L1.8 12.4V5.6L10 0.8Z"
                  stroke="rgba(7,16,34,0.95)"
                  stroke-width="1.2"
                />
                <path
                  d="M10 2.7L16.3 6.4V11.6L10 15.3L3.7 11.6V6.4L10 2.7Z"
                  fill="rgba(201,168,76,0.35)"
                  stroke="rgba(201,168,76,0.8)"
                  stroke-width="0.9"
                />
              </svg>
            </div>
            <div class="text">
              <h1>NEXUS ZONE</h1>
              <div class="sub">SEZ Registry & Control</div>
            </div>
          </div>
        </div>

        <div class="role-badge">
          <span aria-hidden="true">🛡️</span>
          <span>GOD MODE — Super Admin</span>
        </div>

        <nav class="nav" aria-label="Primary">
          <button class="nav-btn active" data-view="dashboard" type="button">
            <span class="icon" aria-hidden="true">🏠</span>
            <span class="label">Dashboard</span>
          </button>
          <button class="nav-btn" data-view="registry" type="button">
            <span class="icon" aria-hidden="true">🏢</span>
            <span class="label">Business Registry</span>
          </button>
          <button class="nav-btn" data-view="applications" type="button">
            <span class="icon" aria-hidden="true">📋</span>
            <span class="label">Applications & Approvals</span>
          </button>
          <button class="nav-btn" data-view="licenses" type="button">
            <span class="icon" aria-hidden="true">🪪</span>
            <span class="label">License Management</span>
          </button>
          <button class="nav-btn" data-view="investors" type="button">
            <span class="icon" aria-hidden="true">👤</span>
            <span class="label">Investor Profiles</span>
          </button>
          <button class="nav-btn" data-view="infrastructure" type="button">
            <span class="icon" aria-hidden="true">🏗️</span>
            <span class="label">Zone Infrastructure</span>
          </button>
          <button class="nav-btn" data-view="fees" type="button">
            <span class="icon" aria-hidden="true">💰</span>
            <span class="label">Fees & Invoicing</span>
          </button>
          <button class="nav-btn" data-view="compliance" type="button">
            <span class="icon" aria-hidden="true">🔍</span>
            <span class="label">Compliance & Audits</span>
          </button>
          <button class="nav-btn" data-view="analytics" type="button">
            <span class="icon" aria-hidden="true">📊</span>
            <span class="label">Analytics & Reports</span>
          </button>
          <button class="nav-btn" data-view="settings" type="button">
            <span class="icon" aria-hidden="true">⚙️</span>
            <span class="label">System Settings</span>
          </button>
          <button class="nav-btn" data-view="portal" type="button">
            <span class="icon" aria-hidden="true">🌐</span>
            <span class="label">Portal Configuration</span>
          </button>
          <button class="nav-btn" data-view="permissions" type="button">
            <span class="icon" aria-hidden="true">🔐</span>
            <span class="label">Access & Permissions</span>
          </button>
        </nav>
      </aside>

      <div id="sidebarBackdrop" class="sidebar-backdrop" aria-hidden="true"></div>

      <div class="main">
        <div class="mobile-topbar">
          <button id="mobileMenuBtn" class="hamburger" type="button">☰</button>
          <div class="section-title" style="margin:0;font-size:20px;">NEXUS ZONE</div>
          <button id="mobileGodBtn" class="hamburger" type="button" title="Open God Mode Console (G)">⚡</button>
        </div>

        <header class="topbar" aria-label="Top Bar">
          <div class="topbar-left">
            <div class="welcome">
              <div id="topbarTitle" class="title">Welcome back, Administrator</div>
              <div id="topbarDate" class="date">—</div>
            </div>
          </div>

          <div class="topbar-mid">
            <div class="search" role="search">
              <span class="lens" aria-hidden="true">⌕</span>
              <input
                id="globalSearch"
                type="search"
                placeholder="Global Search: companies, investors, licenses, invoices..."
                autocomplete="off"
              />
              <div id="searchResults" class="search-results" role="listbox"></div>
            </div>
          </div>

          <div class="topbar-right">
            <button id="darkToggle" class="icon-btn" type="button" title="Toggle Dark/Light Mode">
              <span id="darkToggleIcon" aria-hidden="true">🌙</span>
              <span class="sr-only">Toggle theme</span>
            </button>

            <div class="notif-wrap">
              <button id="notifBtn" class="icon-btn" type="button" aria-haspopup="true" aria-expanded="false">
                <span aria-hidden="true">🔔</span>
                <span class="sr-only">Notifications</span>
              </button>
              <div id="notifPanel" class="notif-panel" role="menu" aria-label="Notification panel">
                <div class="head">
                  <div class="h">Alerts</div>
                  <div style="color:var(--muted2);font-size:12px;">3</div>
                </div>
                <div id="notifList" class="list"></div>
              </div>
            </div>

            <div class="avatar" title="Administrator" aria-hidden="true">NZ</div>
          </div>
        </header>

        <!-- Floating God Mode Badge -->
        <div id="godFloat" class="god-float" role="button" tabindex="0" aria-label="Open God Mode Console">
          <span class="bolt" aria-hidden="true">⚡</span>
          <span>GOD MODE ACTIVE</span>
        </div>

        <!-- Views -->

        <!-- VIEW 1: DASHBOARD -->
        <section id="view-dashboard" class="view active" data-view="dashboard">
          <div class="section-title fade-stagger">Dashboard</div>
          <div class="subtext fade-stagger visible" style="transition-delay:40ms;">
            Operational pulse across the SEZ registry, approvals, and controls.
          </div>

          <div class="kpi-row" aria-label="Key Performance Indicators">
            <div class="card kpi-card fade-stagger" style="transition-delay:0ms;">
              <div class="label">Total Registered Businesses</div>
              <div class="value" data-count="4821">4,821</div>
              <div class="trend">↗ <span style="color:var(--muted2)">Rolling 30d</span></div>
            </div>
            <div class="card kpi-card fade-stagger" style="transition-delay:60ms;">
              <div class="label">Active Licenses</div>
              <div class="value" data-count="3205">3,205</div>
              <div class="trend">↗ <span style="color:var(--muted2)">Stable throughput</span></div>
            </div>
            <div class="card kpi-card fade-stagger" style="transition-delay:120ms;">
              <div class="label">Pending Approvals</div>
              <div class="value" data-count="47">47</div>
              <div class="trend">⏳ <span style="color:var(--muted2)">Triage required</span></div>
            </div>
            <div class="card kpi-card fade-stagger" style="transition-delay:180ms;">
              <div class="label">Revenue This Month</div>
              <div class="value" data-count="2400000">$2.4M</div>
              <div class="trend">↗ <span style="color:var(--muted2)">Collected & flagged</span></div>
            </div>
            <div class="card kpi-card fade-stagger" style="transition-delay:240ms;">
              <div class="label">New Applications (7d)</div>
              <div class="value" data-count="128">128</div>
              <div class="trend">⚡ <span style="color:var(--muted2)">Inbound surge</span></div>
            </div>
            <div class="card kpi-card fade-stagger" style="transition-delay:300ms;">
              <div class="label">Suspended Entities</div>
              <div class="value" data-count="12">12</div>
              <div class="trend">⛔ <span style="color:var(--muted2)">Control enforced</span></div>
            </div>
          </div>

          <div class="grid-2">
            <div class="card list-card fade-stagger" style="transition-delay:80ms;">
              <div class="card-head">
                <div class="h">Recent Activity Feed</div>
                <div class="meta">Live-style mock stream</div>
              </div>
              <div id="activityFeed" class="feed" role="list"></div>
            </div>

            <div class="card list-card fade-stagger" style="transition-delay:120ms;">
              <div class="card-head">
                <div class="h">Approval Queue</div>
                <div class="meta">Top 5 pending</div>
              </div>
              <div class="table-wrap" style="max-height:360px;">
                <table aria-label="Approval queue">
                  <thead>
                    <tr>
                      <th>Company</th>
                      <th>Type</th>
                      <th>Submitted</th>
                      <th>Priority</th>
                      <th>Actions</th>
                    </tr>
                  </thead>
                  <tbody id="approvalQueue"></tbody>
                </table>
              </div>
            </div>
          </div>

          <div class="card map-card fade-stagger" style="transition-delay:140ms;">
            <div class="map-wrap">
              <div class="map-meta">
                <div>
                  <div class="section-title" style="margin:0;font-size:22px;">Zone Activity Map</div>
                  <div class="subtext" style="margin:6px 0 0 0;">SVG floor sectors colored by occupancy</div>
                </div>
                <div class="legend" aria-label="Occupancy legend">
                  <div class="leg">
                    <span class="swatch green"></span> &lt; 50%
                  </div>
                  <div class="leg">
                    <span class="swatch amber"></span> 50-80%
                  </div>
                  <div class="leg">
                    <span class="swatch red"></span> &gt; 80%
                  </div>
                </div>
              </div>

              <svg class="map-svg" viewBox="0 0 860 260" xmlns="http://www.w3.org/2000/svg" aria-label="Zone map">
                <g id="zoneSectors">
                  <path id="zone-A" class="zone-sector" data-zone="Zone A" data-occupancy="0.87"
                    d="M80 40 H340 V120 H80 Z"
                    fill="rgba(47,227,139,0.24)" stroke="rgba(201,168,76,0.55)" stroke-width="2" />

                  <path id="zone-B" class="zone-sector" data-zone="Zone B" data-occupancy="0.62"
                    d="M350 40 H560 V120 H350 Z"
                    fill="rgba(242,184,75,0.22)" stroke="rgba(201,168,76,0.45)" stroke-width="2" />

                  <path id="zone-C" class="zone-sector" data-zone="Zone C" data-occupancy="0.45"
                    d="M80 130 H270 V210 H80 Z"
                    fill="rgba(47,227,139,0.20)" stroke="rgba(201,168,76,0.35)" stroke-width="2" />

                  <path id="zone-D" class="zone-sector" data-zone="Tech Cluster" data-occupancy="0.91"
                    d="M290 130 H560 V210 H290 Z"
                    fill="rgba(227,93,106,0.22)" stroke="rgba(201,168,76,0.55)" stroke-width="2" />

                  <path id="zone-E" class="zone-sector" data-zone="Logistics Hub" data-occupancy="0.78"
                    d="M570 40 H780 V120 H570 Z"
                    fill="rgba(242,184,75,0.22)" stroke="rgba(201,168,76,0.45)" stroke-width="2" />

                  <path id="zone-F" class="zone-sector" data-zone="Industrial Area" data-occupancy="0.53"
                    d="M620 130 H820 V210 H620 Z"
                    fill="rgba(242,184,75,0.18)" stroke="rgba(201,168,76,0.35)" stroke-width="2" />

                  <text x="210" y="85" font-family="JetBrains Mono, monospace" font-size="14" fill="rgba(234,240,255,0.9)" text-anchor="middle">
                    Trading Zone
                  </text>
                  <text x="455" y="85" font-family="JetBrains Mono, monospace" font-size="14" fill="rgba(234,240,255,0.9)" text-anchor="middle">
                    Financial Services
                  </text>
                  <text x="175" y="175" font-family="JetBrains Mono, monospace" font-size="14" fill="rgba(234,240,255,0.9)" text-anchor="middle">
                    Tech Cluster
                  </text>
                  <text x="425" y="175" font-family="JetBrains Mono, monospace" font-size="14" fill="rgba(234,240,255,0.9)" text-anchor="middle">
                    Logistics Hub
                  </text>
                  <text x="675" y="85" font-family="JetBrains Mono, monospace" font-size="14" fill="rgba(234,240,255,0.9)" text-anchor="middle">
                    Tech Cluster
                  </text>
                  <text x="720" y="175" font-family="JetBrains Mono, monospace" font-size="14" fill="rgba(234,240,255,0.9)" text-anchor="middle">
                    Industrial Area
                  </text>
                </g>
              </svg>

              <div class="zone-details" id="zoneDetails">
                <div class="occupancy-bar">
                  <div class="row">
                    <div class="name">Select a sector</div>
                    <div class="pct">Occupancy & breakdown</div>
                  </div>
                  <div class="bar-track">
                    <div id="zoneBarFill" class="bar-fill" style="width:0%"></div>
                  </div>
                  <div style="margin-top:10px;color:var(--muted2);font-size:12px;" id="zoneBreakdown">—</div>
                </div>
                <div class="util-grid">
                  <div class="uhead">
                    <div class="name">Utilities</div>
                    <div class="pct" id="zoneTenantCount">—</div>
                  </div>
                  <div class="items" id="zoneUtilities"></div>
                </div>
              </div>
            </div>
          </div>
        </section>

        <!-- VIEW 2: BUSINESS REGISTRY -->
        <section id="view-registry" class="view" data-view="registry">
          <div class="section-title fade-stagger">Business Registry</div>
          <div class="subtext fade-stagger" style="transition-delay:50ms;">Search, filter, sort, and open profiles via the right-side drawer.</div>

          <div class="toolbar fade-stagger" style="transition-delay:80ms;">
            <div class="filters">
              <div class="field" style="min-width:240px;">
                <div class="lab">Search</div>
                <input id="registrySearch" type="text" placeholder="Name, Reg#, Trade Name..." />
              </div>
              <div class="field">
                <div class="lab">Zone</div>
                <select id="registryZone">
                  <option value="all">All Zones</option>
                </select>
              </div>
              <div class="field">
                <div class="lab">Entity Type</div>
                <select id="registryEntityType">
                  <option value="all">All Types</option>
                </select>
              </div>
              <div class="field">
                <div class="lab">Status</div>
                <select id="registryStatus">
                  <option value="all">All Status</option>
                </select>
              </div>
              <div class="field">
                <div class="lab">Industry</div>
                <select id="registryIndustry">
                  <option value="all">All Industries</option>
                </select>
              </div>
              <div class="field">
                <div class="lab">Year</div>
                <select id="registryYear">
                  <option value="all">All Years</option>
                </select>
              </div>
            </div>

            <div class="filters" style="justify-content:flex-end;">
              <button id="registerNewBtn" class="btn primary" type="button">+ Register New Business</button>
              <button id="exportCsvBtn" class="btn" type="button">Export CSV</button>
              <div class="field" style="min-width:170px;">
                <div class="lab">Bulk Action</div>
                <select id="bulkAction">
                  <option value="none">Bulk Action ▾</option>
                  <option value="suspend">Suspend Selected</option>
                  <option value="renew">Renew Licenses (Selected)</option>
                  <option value="export">Export Selected</option>
                </select>
              </div>
              <button id="applyBulkBtn" class="btn blue" type="button" style="align-self:end;">Apply</button>
            </div>
          </div>

          <div class="table-wrap fade-stagger" style="transition-delay:110ms; max-height:560px;">
            <table aria-label="Business registry table">
              <thead>
                <tr>
                  <th style="width:46px;">
                    <input id="selectAllRegistry" type="checkbox" aria-label="Select all rows" />
                  </th>
                  <th class="sortable" data-sort="regNo">Reg#</th>
                  <th class="sortable" data-sort="companyName">Company Name</th>
                  <th class="sortable" data-sort="entityType">Entity Type</th>
                  <th class="sortable" data-sort="industry">Industry</th>
                  <th class="sortable" data-sort="zone">Zone</th>
                  <th class="sortable" data-sort="licenseExp">License Exp</th>
                  <th class="sortable" data-sort="status">Status</th>
                  <th>Shareholders</th>
                  <th>Action</th>
                </tr>
              </thead>
              <tbody id="registryTbody"></tbody>
            </table>
          </div>
        </section>

        <!-- VIEW 3: APPLICATIONS & APPROVALS -->
        <section id="view-applications" class="view" data-view="applications">
          <div class="section-title fade-stagger">Applications & Approvals</div>
          <div class="subtext fade-stagger" style="transition-delay:50ms;">Kanban overview with detailed table below.</div>

          <div class="kanban fade-stagger" style="transition-delay:80ms; margin-top:10px;">
            <div class="kan-col">
              <div class="hc"><div class="t">SUBMITTED</div><div class="count" id="kanCount-submitted">—</div></div>
              <div id="kanSubmitted"></div>
            </div>
            <div class="kan-col">
              <div class="hc"><div class="t">UNDER REVIEW</div><div class="count" id="kanCount-review">—</div></div>
              <div id="kanReview"></div>
            </div>
            <div class="kan-col">
              <div class="hc"><div class="t">DOCS PENDING</div><div class="count" id="kanCount-docspending">—</div></div>
              <div id="kanDocsPending"></div>
            </div>
            <div class="kan-col">
              <div class="hc"><div class="t">APPROVED</div><div class="count" id="kanCount-approved">—</div></div>
              <div id="kanApproved"></div>
            </div>
            <div class="kan-col">
              <div class="hc"><div class="t">REJECTED</div><div class="count" id="kanCount-rejected">—</div></div>
              <div id="kanRejected"></div>
            </div>
          </div>

          <div class="subtext" style="margin-top:18px;">All applications with SLA countdown progress.</div>
          <div class="table-wrap fade-stagger" style="transition-delay:120ms; max-height:560px;">
            <table aria-label="Applications table">
              <thead>
                <tr>
                  <th>App ID</th>
                  <th>Applicant</th>
                  <th>Type</th>
                  <th>Submitted</th>
                  <th>Reviewer</th>
                  <th>SLA</th>
                  <th>Status</th>
                  <th>Action</th>
                </tr>
              </thead>
              <tbody id="appsTbody"></tbody>
            </table>
          </div>
        </section>

        <!-- VIEW 4: LICENSE MANAGEMENT -->
        <section id="view-licenses" class="view" data-view="licenses">
          <div class="section-title fade-stagger">License Management</div>
          <div class="subtext fade-stagger" style="transition-delay:50ms;">Renew, override, and control license lifecycle (mock data).</div>

          <div class="kpi-row fade-stagger" style="transition-delay:80ms;">
            <div class="card kpi-card">
              <div class="label">Total Licenses</div>
              <div class="value">3,205</div>
              <div class="trend">— <span style="color:var(--muted2)">Registry-wide</span></div>
            </div>
            <div class="card kpi-card">
              <div class="label">Expiring in 30 days</div>
              <div class="value">89</div>
              <div class="trend">⏳ <span style="color:var(--muted2)">Renewal window</span></div>
            </div>
            <div class="card kpi-card">
              <div class="label">Expired</div>
              <div class="value">34</div>
              <div class="trend">⛔ <span style="color:var(--muted2)">Enforcement</span></div>
            </div>
            <div class="card kpi-card">
              <div class="label">Suspended</div>
              <div class="value">12</div>
              <div class="trend">⛔ <span style="color:var(--muted2)">Control active</span></div>
            </div>
            <div class="card kpi-card">
              <div class="label">Processing Renewals</div>
              <div class="value">47</div>
              <div class="trend">🔵 <span style="color:var(--muted2)">In flight</span></div>
            </div>
            <div class="card kpi-card">
              <div class="label">Compliance Holds</div>
              <div class="value">18</div>
              <div class="trend">🧾 <span style="color:var(--muted2)">Docs required</span></div>
            </div>
          </div>

          <div class="filter-cards fade-stagger" style="transition-delay:90ms;">
            <div class="fcard active" data-license-type="all" role="button" tabindex="0">All</div>
            <div class="fcard" data-license-type="Trading" role="button" tabindex="0">Trading</div>
            <div class="fcard" data-license-type="Services" role="button" tabindex="0">Services</div>
            <div class="fcard" data-license-type="Industrial" role="button" tabindex="0">Industrial</div>
            <div class="fcard" data-license-type="Financial" role="button" tabindex="0">Financial</div>
            <div class="fcard" data-license-type="Media" role="button" tabindex="0">Media</div>
            <div class="fcard" data-license-type="Healthcare" role="button" tabindex="0">Healthcare</div>
            <div class="fcard" data-license-type="Technology" role="button" tabindex="0">Technology</div>
            <div class="fcard" data-license-type="Logistics" role="button" tabindex="0">Logistics</div>
            <div class="fcard" data-license-type="Consulting" role="button" tabindex="0">Consulting</div>
            <div class="fcard" data-license-type="Food & Beverage" role="button" tabindex="0">Food &amp; Beverage</div>
          </div>

          <div class="god-panel fade-stagger" style="transition-delay:100ms; margin-bottom:14px;">
            <div class="ghead">
              <div class="t">⚡ Authority Override</div>
              <div class="note">Override actions require PIN confirmation.</div>
            </div>
            <div class="controls">
              <button class="btn primary" type="button" data-requires-pin="true" data-pin-action="Bulk Renew Selected">Bulk Renew Selected</button>
              <div class="field" style="min-width:220px;">
                <div class="lab">Override Expiry Date</div>
                <input id="overrideExpiry" type="date" />
              </div>
              <button class="btn danger" type="button" data-requires-pin="true" data-pin-action="Force Suspend">Force Suspend</button>
              <button class="btn blue" type="button" data-requires-pin="true" data-pin-action="Reinstate License">Reinstate License</button>
              <button class="btn" type="button" data-requires-pin="true" data-pin-action="Generate Batch Certificates">Generate Batch Certificates</button>
              <button class="btn" type="button" data-requires-pin="true" data-pin-action="Mass Email Renewal Notices">Mass Email Renewal Notices</button>
            </div>
          </div>

          <div class="table-wrap fade-stagger" style="transition-delay:120ms; max-height:560px;">
            <table aria-label="License management table">
              <thead>
                <tr>
                  <th style="width:46px;">
                    <input id="selectAllLicenses" type="checkbox" aria-label="Select all licenses" />
                  </th>
                  <th>License #</th>
                  <th>Company</th>
                  <th>Type</th>
                  <th>Activities</th>
                  <th>Issue Date</th>
                  <th>Expiry</th>
                  <th>Renewal Status</th>
                  <th>Action</th>
                </tr>
              </thead>
              <tbody id="licensesTbody"></tbody>
            </table>
          </div>
        </section>

        <!-- VIEW 5: INVESTOR PROFILES -->
        <section id="view-investors" class="view" data-view="investors">
          <div class="section-title fade-stagger">Investor Profiles</div>
          <div class="subtext fade-stagger" style="transition-delay:50ms;">Investor/Director KYC registry (mock data).</div>

          <div class="investor-grid" id="investorGrid"></div>
        </section>

        <!-- VIEW 6: ZONE INFRASTRUCTURE -->
        <section id="view-infrastructure" class="view" data-view="infrastructure">
          <div class="section-title fade-stagger">Zone Infrastructure</div>
          <div class="subtext fade-stagger" style="transition-delay:50ms;">SVG zone map + facilities registry (mock data).</div>

          <div class="card map-card fade-stagger" style="transition-delay:80ms;">
            <div class="map-wrap">
              <div class="map-meta">
                <div>
                  <div class="section-title" style="margin:0;font-size:22px;">Interactive Zone Map</div>
                  <div class="subtext" style="margin:6px 0 0 0;">Click a zone to view occupancy + tenants + utilities</div>
                </div>
              </div>

              <div style="display:grid;grid-template-columns:1fr;gap:14px;">
                <svg class="map-svg" viewBox="0 0 860 260" xmlns="http://www.w3.org/2000/svg" aria-label="Infrastructure map">
                  <g id="infraZoneSectors"></g>
                </svg>
              </div>

              <div class="zone-details" style="margin-top:14px;">
                <div class="occupancy-bar">
                  <div class="row">
                    <div class="name" id="infraZoneName">Zone A</div>
                    <div class="pct" id="infraZonePct">87% occupied</div>
                  </div>
                  <div class="bar-track">
                    <div id="infraZoneBarFill" class="bar-fill" style="width:87%"></div>
                  </div>
                  <div style="margin-top:10px;color:var(--muted2);font-size:12px;" id="infraUnitBreakdown">
                    office/warehouse/land breakdown
                  </div>
                </div>
                <div class="util-grid">
                  <div class="uhead">
                    <div class="name">Tenant & Utilities</div>
                    <div class="pct" id="infraTenantCount">—</div>
                  </div>
                  <div class="items" id="infraUtilities"></div>
                <div style="margin-top:12px;color:var(--muted2);font-size:12px;" id="infraTenantDetails">
                  Tenants: — • Available units: —
                </div>
                </div>
              </div>
            </div>
          </div>

          <div class="god-panel fade-stagger" style="transition-delay:100ms; margin:14px 0 14px 0;">
            <div class="ghead">
              <div class="t">⚡ Authority Override (Zone Controls)</div>
              <div class="note">Freeze, override, reassign, and emergency flags require PIN.</div>
            </div>
            <div class="controls">
              <button class="btn" type="button" data-requires-pin="true" data-pin-action="Freeze New Allocations">Freeze New Allocations</button>
              <button class="btn primary" type="button" data-requires-pin="true" data-pin-action="Override Lease Terms">Override Lease Terms</button>
              <button class="btn blue" type="button" data-requires-pin="true" data-pin-action="Bulk Reassign Units">Bulk Reassign Units</button>
              <div class="field" style="min-width:200px;">
                <div class="lab">Zone Capacity Override</div>
                <input id="capacityOverride" type="number" placeholder="e.g., 9500" />
              </div>
              <button class="btn danger" type="button" data-requires-pin="true" data-pin-action="Emergency Evacuation Flag">Emergency Evacuation Flag</button>
            </div>
          </div>

          <div class="table-wrap fade-stagger" style="transition-delay:120ms; max-height:520px;">
            <table aria-label="Unit and facility registry">
              <thead>
                <tr>
                  <th>Unit ID</th>
                  <th>Type</th>
                  <th>Area (sqft)</th>
                  <th>Tenant</th>
                  <th>Lease Start</th>
                  <th>Lease End</th>
                  <th>Rent/Year</th>
                  <th>Status</th>
                </tr>
              </thead>
              <tbody id="unitsTbody"></tbody>
            </table>
          </div>
        </section>

        <!-- VIEW 7: FEES & INVOICING -->
        <section id="view-fees" class="view" data-view="fees">
          <div class="section-title fade-stagger">Fees & Invoicing</div>
          <div class="subtext fade-stagger" style="transition-delay:50ms;">Revenue breakdown, receivables, and invoice lifecycle (mock).</div>

          <div class="grid-2">
            <div class="card fade-stagger" style="transition-delay:80ms;">
              <div class="card-head">
                <div class="h">Monthly Revenue</div>
                <div class="meta">Last 12 months (CSS bars)</div>
              </div>
              <div class="bars" id="revenueBars" aria-label="Revenue bars chart"></div>
              <div style="margin-top:8px;color:var(--muted2);font-size:12px;">
                Tip: This is CSS-only bars; values are hardcoded in JS for sizing.
              </div>
            </div>
            <div class="card fade-stagger" style="transition-delay:120ms;">
              <div class="card-head">
                <div class="h">Fee Collection Mix</div>
                <div class="meta">Donut chart distribution</div>
              </div>
              <div style="display:flex;align-items:center;justify-content:space-between;gap:12px;flex-wrap:wrap;">
                <div class="donut" aria-label="Fee mix donut">
                  <div class="center">
                    <div class="a">100%</div>
                    <div class="b">Mix</div>
                  </div>
                </div>
                <div style="flex:1;min-width:220px;">
                  <div style="display:flex;flex-wrap:wrap;gap:10px;margin-bottom:10px;">
                    <div class="pill" style="border-color:rgba(201,168,76,0.35);background:rgba(201,168,76,0.08);color:var(--gold);">
                      License Fees 40%
                    </div>
                    <div class="pill" style="border-color:rgba(77,163,255,0.35);background:rgba(77,163,255,0.08);color:rgba(77,163,255,1);">
                      Visa Fees 20%
                    </div>
                    <div class="pill" style="border-color:rgba(242,184,75,0.35);background:rgba(242,184,75,0.08);color:rgba(242,184,75,1);">
                      Registration 15%
                    </div>
                    <div class="pill" style="border-color:rgba(227,93,106,0.35);background:rgba(227,93,106,0.08);color:rgba(227,93,106,1);">
                      Penalties 8%
                    </div>
                    <div class="pill" style="border-color:rgba(120,150,255,0.28);background:rgba(120,150,255,0.08);color:rgba(120,150,255,1);">
                      Other 17%
                    </div>
                  </div>
                  <div class="kv-grid" style="grid-template-columns:1fr;gap:10px;">
                    <div class="kv">
                      <div class="k">Outstanding receivables</div>
                      <div class="v">$847,000</div>
                    </div>
                    <div class="kv">
                      <div class="k">Overdue (>90 days)</div>
                      <div class="v">$123,000</div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <div class="god-panel fade-stagger" style="transition-delay:100ms; margin-top:14px; margin-bottom:14px;">
            <div class="ghead">
              <div class="t">⚡ Authority Override (Fee Controls)</div>
              <div class="note">Discounts, fee overrides, and bulk invoice actions require PIN.</div>
            </div>
            <div class="controls">
              <button class="btn primary" type="button" data-requires-pin="true" data-pin-action="Apply Discount/Waiver">Apply Discount/Waiver</button>
              <button class="btn" type="button" data-requires-pin="true" data-pin-action="Override Fee Schedule">Override Fee Schedule</button>
              <button class="btn blue" type="button" data-requires-pin="true" data-pin-action="Force Mark Paid">Force Mark Paid</button>
              <button class="btn" type="button" data-requires-pin="true" data-pin-action="Bulk Generate Invoices">Bulk Generate Invoices</button>
              <button class="btn danger" type="button" data-requires-pin="true" data-pin-action="Apply Penalty Override">Apply Penalty Override</button>
            </div>
          </div>

          <div class="grid-2">
            <div class="table-wrap fade-stagger" style="transition-delay:120ms; max-height:520px;">
              <table aria-label="Fee schedule">
                <thead>
                  <tr>
                    <th>Service</th>
                    <th>Entity Type</th>
                    <th>Fee (USD)</th>
                    <th>VAT</th>
                    <th>Total</th>
                    <th>Action</th>
                  </tr>
                </thead>
                <tbody id="feeScheduleTbody"></tbody>
              </table>
            </div>

            <div class="table-wrap fade-stagger" style="transition-delay:140ms; max-height:520px;">
              <table aria-label="Invoice management">
                <thead>
                  <tr>
                    <th>Invoice #</th>
                    <th>Company</th>
                    <th>Service</th>
                    <th>Amount</th>
                    <th>Due Date</th>
                    <th>Status</th>
                    <th>Action</th>
                  </tr>
                </thead>
                <tbody id="invoicesTbody"></tbody>
              </table>
            </div>
          </div>
        </section>

        <!-- VIEW 8: COMPLIANCE & AUDITS -->
        <section id="view-compliance" class="view" data-view="compliance">
          <div class="section-title fade-stagger">Compliance & Audits</div>
          <div class="subtext fade-stagger" style="transition-delay:50ms;">Risk matrix + health score gauge + audit trail (mock).</div>

          <div class="grid-2">
            <div class="card fade-stagger" style="transition-delay:80ms;">
              <div class="card-head">
                <div class="h">Compliance Risk Dashboard</div>
                <div class="meta">Likelihood vs Impact (3x3)</div>
              </div>
              <div class="risk-matrix" id="riskMatrix"></div>
            </div>
            <div class="card fade-stagger" style="transition-delay:110ms;">
              <div class="card-head">
                <div class="h">Zone Overall</div>
                <div class="meta">Compliance health</div>
              </div>
              <div style="display:flex;gap:14px;align-items:center;justify-content:space-between;flex-wrap:wrap;">
                <div class="gauge" style="--deg:310deg;">
                  <div class="ring" style="--deg:310deg;">
                    <div class="txt">
                      87
                      <div class="sub">/100</div>
                    </div>
                  </div>
                </div>
                <div style="flex:1;min-width:220px;">
                  <div class="kv-grid" style="grid-template-columns:1fr;gap:10px;">
                    <div class="kv">
                      <div class="k">Flags requiring review</div>
                      <div class="v" style="color:var(--amber);">22</div>
                    </div>
                    <div class="kv">
                      <div class="k">Active compliance freezes</div>
                      <div class="v" style="color:var(--red);">4</div>
                    </div>
                    <div class="kv">
                      <div class="k">Pending SAR drafts</div>
                      <div class="v" style="color:var(--blue);">6</div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <div class="god-panel fade-stagger" style="transition-delay:130ms; margin-top:14px; margin-bottom:14px;">
            <div class="ghead">
              <div class="t">⚡ Authority Override (Compliance Controls)</div>
              <div class="note">Audit, escalation, freezes, SAR and overrides require PIN.</div>
            </div>
            <div class="controls">
              <button class="btn primary" type="button" data-requires-pin="true" data-pin-action="Trigger Full Audit">Trigger Full Audit</button>
              <button class="btn" type="button" data-requires-pin="true" data-pin-action="Escalate to Regulator">Escalate to Regulator</button>
              <button class="btn danger" type="button" data-requires-pin="true" data-pin-action="Freeze Entity Operations">Freeze Entity Operations</button>
              <button class="btn blue" type="button" data-requires-pin="true" data-pin-action="Generate SAR">Generate Suspicious Activity Report (SAR)</button>
              <button class="btn" type="button" data-requires-pin="true" data-pin-action="Override Compliance Status">Override Compliance Status</button>
              <button class="btn danger" type="button" data-requires-pin="true" data-pin-action="Initiate Deregistration Proceedings">Initiate Deregistration Proceedings</button>
            </div>
          </div>

          <div class="grid-2">
            <div class="table-wrap fade-stagger" style="transition-delay:150ms; max-height:520px;">
              <table aria-label="Flagged companies list">
                <thead>
                  <tr>
                    <th>Company</th>
                    <th>Flag Reason</th>
                    <th>Severity</th>
                    <th>Since</th>
                    <th>Assigned To</th>
                    <th>Status</th>
                  </tr>
                </thead>
                <tbody id="flaggedTbody"></tbody>
              </table>
            </div>

            <div class="table-wrap fade-stagger" style="transition-delay:170ms; max-height:520px;">
              <table aria-label="Audit log">
                <thead>
                  <tr>
                    <th>Timestamp</th>
                    <th>Action</th>
                    <th>Entity</th>
                    <th>Changed By</th>
                    <th>Before</th>
                    <th>After</th>
                    <th>IP Address</th>
                  </tr>
                </thead>
                <tbody id="auditTbody"></tbody>
              </table>
            </div>
          </div>
        </section>

        <!-- VIEW 9: ANALYTICS & REPORTS -->
        <section id="view-analytics" class="view" data-view="analytics">
          <div class="section-title fade-stagger">Analytics & Reports</div>
          <div class="subtext fade-stagger" style="transition-delay:50ms;">Report builder, pre-built cards, and CSS visualizations (mock).</div>

          <div class="card fade-stagger" style="transition-delay:80px; margin-bottom:14px;">
            <div class="card-head">
              <div class="h">Report Builder Panel</div>
              <div class="meta">Generate scheduled outputs</div>
            </div>
            <div class="toolbar" style="margin-bottom:0;">
              <div class="filters">
                <div class="field" style="min-width:200px;">
                  <div class="lab">Date range</div>
                  <input id="reportStart" type="date" />
                </div>
                <div class="field" style="min-width:200px;">
                  <div class="lab">To</div>
                  <input id="reportEnd" type="date" />
                </div>
                <div class="field">
                  <div class="lab">Entity type</div>
                  <select id="reportEntityType">
                    <option value="all">All</option>
                  </select>
                </div>
                <div class="field">
                  <div class="lab">Zone</div>
                  <select id="reportZone">
                    <option value="all">All</option>
                  </select>
                </div>
              </div>
              <div class="filters" style="justify-content:flex-end;">
                <div class="field" style="min-width:250px;">
                  <div class="lab">Report type</div>
                  <select id="reportType">
                    <option value="Business Growth">Business Growth</option>
                    <option value="Revenue">Revenue</option>
                    <option value="License Expiry">License Expiry</option>
                    <option value="Visa Utilization">Visa Utilization</option>
                    <option value="Compliance Summary">Compliance Summary</option>
                    <option value="Investor Nationality">Investor Nationality</option>
                  </select>
                </div>
                <button class="btn primary" type="button" id="generateReportBtn">Generate</button>
              </div>
            </div>
          </div>

          <div class="investor-grid" style="grid-template-columns: repeat(3, minmax(260px, 1fr));">
            <div id="reportCards"></div>
          </div>

          <div class="grid-2" style="margin-top:14px;">
            <div class="card fade-stagger" style="transition-delay:120ms;">
              <div class="card-head">
                <div class="h">Registration Trend</div>
                <div class="meta">12 months (CSS-only line-ish bars)</div>
              </div>
              <div class="bars" id="trendChart"></div>
            </div>
            <div class="card fade-stagger" style="transition-delay:150ms;">
              <div class="card-head">
                <div class="h">Industry Breakdown</div>
                <div class="meta">Horizontal bars</div>
              </div>
              <div id="industryBars" style="padding:0 10px 6px 10px;"></div>
            </div>
          </div>

          <div class="grid-2" style="margin-top:14px;">
            <div class="card fade-stagger" style="transition-delay:170ms;">
              <div class="card-head">
                <div class="h">Nationality Heatmap</div>
                <div class="meta">Top 10 investor nationalities</div>
              </div>
              <div id="nationalityHeatmap" style="padding:0 12px 6px 12px;"></div>
            </div>
            <div class="card fade-stagger" style="transition-delay:190ms;">
              <div class="card-head">
                <div class="h">Revenue vs Target</div>
                <div class="meta">Comparison bars</div>
              </div>
              <div id="revenueVsTarget" style="padding:0 12px 6px 12px;"></div>
            </div>
          </div>
        </section>

        <!-- VIEW 10: SYSTEM SETTINGS -->
        <section id="view-settings" class="view" data-view="settings">
          <div class="section-title fade-stagger">System Settings</div>
          <div class="subtext fade-stagger" style="transition-delay:50ms;">Organized configuration sections (mock).</div>

          <div class="grid-2">
            <div class="card fade-stagger" style="transition-delay:80ms;">
              <div class="card-head"><div class="h">Zone Configuration</div><div class="meta">Branding & jurisdiction</div></div>
              <div class="kv-grid">
                <div class="kv"><div class="k">Zone name</div><div class="v">NEXUS ZONE</div></div>
                <div class="kv"><div class="k">Abbreviation</div><div class="v">NZ</div></div>
                <div class="kv"><div class="k">Jurisdiction</div><div class="v">SEZ Authority — DMCC-inspired</div></div>
                <div class="kv"><div class="k">Free zone type</div><div class="v">DMCC-style multi-commodity</div></div>
              </div>
              <div class="quick-actions">
                <div class="qahead">
                  <div class="t">Authority branding</div>
                  <div class="meta" style="color:var(--muted2);font-size:12px;">Logo & templates (mock)</div>
                </div>
                <div class="list">
                  <button class="btn" type="button" data-requires-pin="true" data-pin-action="Save Branding Templates">Save templates</button>
                  <button class="btn" type="button" data-requires-pin="true" data-pin-action="Regenerate Letterhead">Regenerate letterhead</button>
                </div>
              </div>
            </div>

            <div class="card fade-stagger" style="transition-delay:110ms;">
              <div class="card-head"><div class="h">Workflow Settings</div><div class="meta">Approvals & document gates</div></div>
              <div class="kv-grid">
                <div class="kv"><div class="k">Approval SLA timers</div><div class="v">Per application type</div></div>
                <div class="kv"><div class="k">Auto-reminder triggers</div><div class="v">X days before expiry</div></div>
                <div class="kv"><div class="k">Required docs checklist</div><div class="v">Per entity type</div></div>
                <div class="kv"><div class="k">Mandatory KYC fields</div><div class="v">
                  <label class="switch" title="Toggle mandatory KYC fields (mock)">
                    <input type="checkbox" data-switch-key="mandatoryKyc" checked />
                    <span class="track" aria-hidden="true"></span>
                  </label>
                </div></div>
                <div class="kv"><div class="k">API Base URL</div><div class="v">https://api.nexuzzone.ae/v1</div></div>
                <div class="kv"><div class="k">Enable API access keys</div><div class="v">
                  <label class="switch" title="Toggle API keys access (mock)">
                    <input type="checkbox" data-switch-key="apiKeysEnabled" checked />
                    <span class="track" aria-hidden="true"></span>
                  </label>
                  <span style="margin-left:10px;color:var(--muted2);font-size:12px;">•••••••• (masked)</span>
                </div></div>
              </div>
              <div class="quick-actions">
                <div class="qahead">
                  <div class="t">Workflow controls</div>
                  <div class="meta" style="color:var(--muted2);font-size:12px;">Simulated</div>
                </div>
                <div class="list">
                  <button class="btn primary" type="button" data-requires-pin="true" data-pin-action="Save Workflow SLA">Save SLA</button>
                  <button class="btn" type="button">Preview checklist</button>
                </div>
              </div>
            </div>
          </div>

          <div class="grid-2" style="margin-top:14px;">
            <div class="card fade-stagger" style="transition-delay:130ms;">
              <div class="card-head"><div class="h">User Roles & Permissions</div><div class="meta">Role matrix (mock)</div></div>
              <div class="kv-grid">
                <div class="kv"><div class="k">Roles</div><div class="v">Super Admin, Zone Admin, Reviewer, Finance Officer, Compliance Officer, Read Only</div></div>
                <div class="kv"><div class="k">Permission matrix</div><div class="v">Checkboxes per module</div></div>
              </div>
            </div>
            <div class="card fade-stagger" style="transition-delay:150ms;">
              <div class="card-head"><div class="h">System Logs</div><div class="meta">Audit history (mock)</div></div>
              <div class="kv-grid">
                <div class="kv"><div class="k">API request log</div><div class="v">Enabled</div></div>
                <div class="kv"><div class="k">Login history</div><div class="v">Enabled</div></div>
                <div class="kv"><div class="k">Data export history</div><div class="v">Enabled</div></div>
                <div class="kv"><div class="k">Retention</div><div class="v">180 days</div></div>
              </div>
              <div class="quick-actions">
                <div class="qahead"><div class="t">Export</div><div class="meta" style="color:var(--muted2);font-size:12px;">Download simulated logs</div></div>
                <div class="list">
                  <button class="btn" type="button" data-requires-pin="true" data-pin-action="Export API Request Log">Download API logs</button>
                  <button class="btn" type="button" data-requires-pin="true" data-pin-action="Export Login History">Download login history</button>
                </div>
              </div>
            </div>
          </div>
        </section>

        <!-- VIEW 11: PORTAL CONFIGURATION -->
        <section id="view-portal" class="view" data-view="portal">
          <div class="section-title fade-stagger">Portal Configuration</div>
          <div class="subtext fade-stagger" style="transition-delay:50ms;">Public portal toggles, payment and integrations (mock).</div>

          <div class="grid-2">
            <div class="card fade-stagger" style="transition-delay:80ms;">
              <div class="card-head"><div class="h">Public Investor Portal</div><div class="meta">ON/OFF toggle</div></div>
              <div class="kv-grid">
                <div class="kv"><div class="k">Status</div><div class="v">
                  <label class="switch" title="Toggle public investor portal (mock)">
                    <input type="checkbox" data-switch-key="publicPortal" checked />
                    <span class="track" aria-hidden="true"></span>
                  </label>
                </div></div>
                <div class="kv"><div class="k">Access scope</div><div class="v">KYC & invoice status</div></div>
              </div>
              <div class="quick-actions">
                <div class="qahead"><div class="t">Portal controls</div><div class="meta" style="color:var(--muted2);font-size:12px;">Requires PIN</div></div>
                <div class="list">
                  <button class="btn danger" type="button" data-requires-pin="true" data-pin-action="Disable Public Portals">Disable public portals</button>
                  <button class="btn primary" type="button" data-requires-pin="true" data-pin-action="Enable Public Portals">Enable public portals</button>
                </div>
              </div>
            </div>
            <div class="card fade-stagger" style="transition-delay:110ms;">
              <div class="card-head"><div class="h">Integrations</div><div class="meta">Payments & e-Sign</div></div>
              <div class="kv-grid">
                <div class="kv"><div class="k">Online payment gateway</div><div class="v">
                  <label class="switch" title="Toggle online payments (mock)">
                    <input type="checkbox" data-switch-key="onlinePayments" checked />
                    <span class="track" aria-hidden="true"></span>
                  </label>
                </div></div>
                <div class="kv"><div class="k">e-Signature integration</div><div class="v">
                  <label class="switch" title="Toggle e-Signature (mock)">
                    <input type="checkbox" data-switch-key="eSign" checked />
                    <span class="track" aria-hidden="true"></span>
                  </label>
                </div></div>
                <div class="kv"><div class="k">Document auto-generation</div><div class="v">
                  <label class="switch" title="Toggle document auto-generation (mock)">
                    <input type="checkbox" data-switch-key="docAutoGen" checked />
                    <span class="track" aria-hidden="true"></span>
                  </label>
                </div></div>
                <div class="kv"><div class="k">API access keys</div><div class="v">
                  <label class="switch" title="Toggle API keys access (mock)">
                    <input type="checkbox" data-switch-key="apiKeysEnabled" checked />
                    <span class="track" aria-hidden="true"></span>
                  </label>
                  <span style="margin-left:10px;color:var(--muted2);font-size:12px;">•••••••• (masked)</span>
                </div></div>
              </div>
              <div class="quick-actions">
                <div class="qahead"><div class="t">Integration actions</div><div class="meta" style="color:var(--muted2);font-size:12px;">Simulated</div></div>
                <div class="list">
                  <button class="btn" type="button" data-requires-pin="true" data-pin-action="Rotate API Keys">Rotate API keys</button>
                  <button class="btn" type="button" data-requires-pin="true" data-pin-action="Sync Document Templates">Sync templates</button>
                </div>
              </div>
            </div>
          </div>
        </section>

        <!-- VIEW 12: ACCESS & PERMISSIONS (GOD MODE) -->
        <section id="view-permissions" class="view" data-view="permissions">
          <div class="section-title fade-stagger">Access & Permissions</div>
          <div class="subtext fade-stagger" style="transition-delay:50ms;">User management table + authority override console (mock).</div>

          <div class="grid-2">
            <div class="table-wrap fade-stagger" style="transition-delay:80ms; max-height:560px;">
              <table aria-label="User management table">
                <thead>
                  <tr>
                    <th>User</th>
                    <th>Role</th>
                    <th>Last Login</th>
                    <th>2FA</th>
                    <th>Status</th>
                    <th>Action</th>
                  </tr>
                </thead>
                <tbody id="usersTbody"></tbody>
              </table>
            </div>

            <div class="table-wrap fade-stagger" style="transition-delay:100ms; max-height:520px; margin-top:14px;">
              <table aria-label="Permission matrix table">
                <thead>
                  <tr>
                    <th>Module</th>
                    <th>Super Admin</th>
                    <th>Zone Admin</th>
                    <th>Reviewer</th>
                    <th>Finance Officer</th>
                    <th>Compliance Officer</th>
                    <th>Read Only</th>
                  </tr>
                </thead>
                <tbody id="permTbody"></tbody>
              </table>
            </div>

            <div class="god-panel fade-stagger" style="transition-delay:100ms;">
              <div class="ghead">
                <div class="t">⚡ AUTHORITY OVERRIDE CONSOLE</div>
                <div class="note">
                  <span class="warning-badge">⚠️ Irreversible Actions</span>
                </div>
              </div>
              <div style="color:var(--muted2);font-size:12px;margin-bottom:10px;">
                Buttons below will prompt for a simulated confirmation + a PIN (4 digits: <b>1234</b>).
              </div>
              <div class="controls">
                <button class="btn danger" type="button" data-requires-pin="true" data-pin-action="Force Logout All Sessions">Force Logout All Sessions</button>
                <button class="btn primary" type="button" data-requires-pin="true" data-pin-action="Lock / Unlock User Account">Lock / Unlock User Account</button>
                <button class="btn danger" type="button" data-requires-pin="true" data-pin-action="Reset All Permissions to Default">Reset All Permissions to Default</button>
                <button class="btn danger" type="button" data-requires-pin="true" data-pin-action="Enable Maintenance Mode">Enable Maintenance Mode</button>
                <button class="btn primary" type="button" data-requires-pin="true" data-pin-action="Database Backup — Trigger Now">Database Backup — Trigger Now</button>
                <button class="btn danger" type="button" data-requires-pin="true" data-pin-action="Emergency Freeze — All Pending Approvals">Emergency Freeze — All Pending Approvals</button>
                <button class="btn" type="button" data-requires-pin="true" data-pin-action="Audit Trail — Download Full Log">Audit Trail — Download Full Log</button>
                <button class="btn" type="button" data-requires-pin="true" data-pin-action="System Announcement Broadcast">System Announcement Broadcast</button>
                <button class="btn primary" type="button" data-requires-pin="true" data-pin-action="Two-Factor Authentication: Force Enable All">Two-Factor Authentication: Force Enable All</button>
                <button class="btn" type="button" data-requires-pin="true" data-pin-action="API Rate Limit Override">API Rate Limit Override</button>
              </div>
            </div>
          </div>
        </section>

        <!-- Drawer: Business Profile -->
        <div id="drawerBackdrop" class="drawer-backdrop" aria-hidden="true"></div>
        <aside id="businessDrawer" class="drawer" aria-label="Business profile drawer">
          <div class="dhead">
            <div class="left">
              <div id="drawerCompanyName" class="name">—</div>
              <div class="meta">
                <div id="drawerRegBadge" class="reg-badge">Reg# —</div>
                <div id="drawerStatusPill" class="pill active">Active</div>
              </div>
            </div>
            <button id="drawerCloseBtn" class="close-btn" type="button" aria-label="Close drawer">✕</button>
          </div>

          <div class="dbody">
            <div class="tabs" role="tablist" aria-label="Drawer tabs">
              <button class="tab active" data-tab="overview" type="button">Overview</button>
              <button class="tab" data-tab="documents" type="button">Documents</button>
              <button class="tab" data-tab="directors" type="button">Directors</button>
              <button class="tab" data-tab="licenses" type="button">Licenses</button>
              <button class="tab" data-tab="invoices" type="button">Invoices</button>
              <button class="tab" data-tab="compliance" type="button">Compliance</button>
              <button class="tab" data-tab="audit" type="button">Audit Log</button>
            </div>

            <!-- Overview -->
            <div id="drawerOverview" class="drawer-section active">
              <div class="kv-grid">
                <div class="kv"><div class="k">Reg date</div><div class="v" id="drawerRegDate">—</div></div>
                <div class="kv"><div class="k">Activities</div><div class="v" id="drawerActivities">—</div></div>
                <div class="kv"><div class="k">Address</div><div class="v" id="drawerAddress">—</div></div>
                <div class="kv"><div class="k">Capital</div><div class="v" id="drawerCapital">—</div></div>
                <div class="kv"><div class="k">Visa quota</div><div class="v" id="drawerVisaQuota">—</div></div>
                <div class="kv"><div class="k">Bank details</div><div class="v" id="drawerBank">—</div></div>
                <div class="kv"><div class="k">Contact</div><div class="v" id="drawerContact">—</div></div>
                <div class="kv"><div class="k">Trade name</div><div class="v" id="drawerTradeName">—</div></div>
              </div>
            </div>

            <!-- Documents -->
            <div id="drawerDocuments" class="drawer-section">
              <div class="doc-list" id="drawerDocsList"></div>
            </div>

            <!-- Directors -->
            <div id="drawerDirectors" class="drawer-section">
              <div class="doc-list" id="drawerDirectorsList"></div>
            </div>

            <!-- Licenses -->
            <div id="drawerLicenses" class="drawer-section">
              <div class="doc-list" id="drawerLicensesList"></div>
            </div>

            <!-- Invoices -->
            <div id="drawerInvoices" class="drawer-section">
              <div class="doc-list" id="drawerInvoicesList"></div>
            </div>

            <!-- Compliance -->
            <div id="drawerCompliance" class="drawer-section">
              <div class="kv-grid">
                <div class="kv"><div class="k">Compliance status</div><div class="v" id="drawerComplianceStatus">—</div></div>
                <div class="kv"><div class="k">KYC verification</div><div class="v" id="drawerKyc">—</div></div>
                <div class="kv"><div class="k">UBO declaration</div><div class="v" id="drawerUbo">—</div></div>
                <div class="kv"><div class="k">Last audit</div><div class="v" id="drawerLastAudit">—</div></div>
              </div>
              <div class="quick-actions">
                <div class="qahead"><div class="t">Flagged items</div><div class="meta" style="color:var(--muted2);font-size:12px;">Mock list</div></div>
                <div class="doc-list" id="drawerComplianceFlags"></div>
              </div>
            </div>

            <!-- Audit Log -->
            <div id="drawerAudit" class="drawer-section">
              <div class="table-wrap" style="max-height:320px;">
                <table aria-label="Company audit log">
                  <thead>
                    <tr>
                      <th>Timestamp</th>
                      <th>Action</th>
                      <th>Before</th>
                      <th>After</th>
                      <th>IP</th>
                    </tr>
                  </thead>
                  <tbody id="drawerAuditTbody"></tbody>
                </table>
              </div>
            </div>

            <div class="quick-actions">
              <div class="qahead">
                <div class="t">Quick actions</div>
                <div class="meta" style="color:var(--muted2);font-size:12px;">Overrides require PIN</div>
              </div>
              <div class="list">
                <button class="btn primary small" type="button" data-action="Approve" data-requires-pin="false">Approve</button>
                <button class="btn danger small" type="button" data-action="Suspend" data-requires-pin="true">Suspend</button>
                <button class="btn blue small" type="button" data-action="Renew License" data-requires-pin="false">Renew License</button>
                <button class="btn small" type="button" data-action="Send Notice" data-requires-pin="false">Send Notice</button>
                <button class="btn small" type="button" data-action="Export Profile" data-requires-pin="false">Export Profile</button>
                <button class="btn small" type="button" data-action="Flag for Audit" data-requires-pin="false">Flag for Audit</button>
                <button class="btn danger small" type="button" data-action="Delete" data-requires-pin="true">Delete</button>
              </div>
            </div>
          </div>
        </aside>

        <!-- Investor modal -->
        <div id="investorModalBackdrop" class="modal-backdrop" aria-hidden="true">
          <div id="investorModal" class="modal" role="dialog" aria-label="Investor profile modal">
            <div class="mhead">
              <div>
                <div class="h" id="investorModalName">—</div>
                <div class="pin-hint" id="investorModalKycSummary">—</div>
              </div>
              <button id="investorModalClose" class="x" type="button" aria-label="Close">✕</button>
            </div>
            <div class="mbody">
              <div class="tabs" role="tablist" aria-label="Investor modal tabs">
                <button class="tab active" data-itab="personal" type="button">Personal info</button>
                <button class="tab" data-itab="entities" type="button">Associated Entities</button>
                <button class="tab" data-itab="documents" type="button">Documents</button>
                <button class="tab" data-itab="ubo" type="button">Beneficial Ownership</button>
              </div>

              <div id="investorTabPersonal" class="drawer-section active">
                <div class="kv-grid">
                  <div class="kv"><div class="k">DOB</div><div class="v" id="investorDOB">—</div></div>
                  <div class="kv"><div class="k">Nationality</div><div class="v" id="investorNationality">—</div></div>
                  <div class="kv"><div class="k">Passport #</div><div class="v" id="investorPassport">—</div></div>
                  <div class="kv"><div class="k">Visa status</div><div class="v" id="investorVisaStatus">—</div></div>
                </div>
              </div>

              <div id="investorTabEntities" class="drawer-section">
                <div class="table-wrap" style="max-height:320px;">
                  <table aria-label="Associated entities">
                    <thead>
                      <tr>
                        <th>Company</th>
                        <th>Role</th>
                      </tr>
                    </thead>
                    <tbody id="investorEntitiesTbody"></tbody>
                  </table>
                </div>
              </div>

              <div id="investorTabDocuments" class="drawer-section">
                <div class="doc-list" id="investorDocsList"></div>
              </div>

              <div id="investorTabUbo" class="drawer-section">
                <div class="kv-grid">
                  <div class="kv"><div class="k">UBO declaration</div><div class="v" id="investorUboStatus">—</div></div>
                  <div class="kv"><div class="k">Submission</div><div class="v" id="investorUboDate">—</div></div>
                </div>
                <div class="quick-actions" style="margin-top:14px;">
                  <div class="qahead">
                    <div class="t">GOD MODE</div>
                    <div class="meta" style="color:var(--muted2);font-size:12px;">PIN required</div>
                  </div>
                  <div class="list">
                    <button class="btn primary small" type="button" data-requires-pin="true" data-pin-action="Override KYC Status">Override KYC Status</button>
                    <button class="btn danger small" type="button" data-requires-pin="true" data-pin-action="Blacklist Individual">Blacklist Individual</button>
                    <button class="btn small" type="button" data-requires-pin="false" data-action="Generate KYC Report">Generate KYC Report</button>
                    <button class="btn small" type="button" data-requires-pin="false" data-action="Link to External Database">Link to External Database</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- God Mode overlay console -->
        <div id="godOverlayBackdrop" class="modal-backdrop" aria-hidden="true">
          <div class="modal" role="dialog" aria-label="God Mode overlay">
            <div class="mhead">
              <div>
                <div class="h">⚡ GOD MODE ACTIVE — Authority Console</div>
                <div class="pin-hint">Press `G` to open/close. Overrides require PIN confirmation.</div>
              </div>
              <button id="godOverlayClose" class="x" type="button" aria-label="Close God Mode">✕</button>
            </div>
            <div class="mbody">
              <div class="god-panel" style="background:transparent;box-shadow:none;margin:0;border:none;padding:0;">
                <div class="controls">
                  <button class="btn danger" type="button" data-requires-pin="true" data-pin-action="Force Logout All Sessions">Force Logout All Sessions</button>
                  <button class="btn primary" type="button" data-requires-pin="true" data-pin-action="Lock / Unlock User Account">Lock / Unlock User Account</button>
                  <button class="btn danger" type="button" data-requires-pin="true" data-pin-action="Emergency Freeze — All Pending Approvals">Emergency Freeze — All Pending Approvals</button>
                  <button class="btn" type="button" data-requires-pin="true" data-pin-action="Audit Trail — Download Full Log">Audit Trail — Download Full Log</button>
                  <button class="btn" type="button" data-requires-pin="true" data-pin-action="System Announcement Broadcast">System Announcement Broadcast</button>
                  <button class="btn primary" type="button" data-requires-pin="true" data-pin-action="Two-Factor Authentication: Force Enable All">Two-Factor Authentication: Force Enable All</button>
                  <button class="btn" type="button" data-requires-pin="true" data-pin-action="API Rate Limit Override">API Rate Limit Override</button>
                  <button class="btn danger" type="button" data-requires-pin="true" data-pin-action="Maintenance Mode">Enable Maintenance Mode</button>
                </div>
              </div>
              <div class="pin-hint" style="margin-top:14px;">
                Notes: This is a static mock UI. Actions show simulated toasts and do not persist changes.
              </div>
            </div>
          </div>
        </div>

        <!-- PIN modal -->
        <div id="pinBackdrop" class="modal-backdrop" aria-hidden="true">
          <div class="modal" role="dialog" aria-label="PIN confirmation modal">
            <div class="mhead">
              <div>
                <div class="h">PIN Confirmation Required</div>
                <div class="pin-hint" id="pinPrompt">Confirm with PIN to proceed.</div>
              </div>
              <button id="pinCloseBtn" class="x" type="button" aria-label="Close">✕</button>
            </div>
            <div class="mbody">
              <div class="pin-hint">Enter 4 digits. Demo PIN: <b>1234</b></div>
              <input id="pinHidden" class="pin-input-hidden" inputmode="numeric" autocomplete="one-time-code" />
              <div class="pin-row" aria-label="PIN digits">
                <div id="pinD1" class="pin-digit active"> </div>
                <div id="pinD2" class="pin-digit"> </div>
                <div id="pinD3" class="pin-digit"> </div>
                <div id="pinD4" class="pin-digit"> </div>
              </div>
              <div class="pin-actions">
                <button id="pinCancelBtn" class="btn" type="button">Cancel</button>
              </div>
            </div>
          </div>
        </div>

        <!-- Context menu -->
        <div id="contextMenu" class="context-menu" role="menu" aria-label="Row context menu">
          <div class="item" data-ctx="view">👁️ View</div>
          <div class="item" data-ctx="edit">✏️ Edit</div>
          <div class="item" data-ctx="suspend">⛔ Suspend</div>
          <div class="item" data-ctx="export">⬇️ Export</div>
          <div class="item" data-ctx="audit">🧾 Audit Trail</div>
          <div class="item" data-ctx="delete">🗑️ Delete</div>
        </div>

        <div id="toastContainer" class="toasts" aria-live="polite"></div>
      </div>
    </div>

    <script>
      // ---------------------------
      // Mock Data (static)
      // ---------------------------
      const statusToPillClass = {
        Active: "active",
        Pending: "pending",
        Suspended: "suspended",
        Processing: "processing",
        Deregistered: "deregistered",
      };

      const mock = {
        activityFeed: [
          { company: "Al Masdar Trading LLC", action: "License Renewed", time: "2 min ago", avatarText: "AM" },
          { company: "GreenTech FZE", action: "Application Submitted", time: "8 min ago", avatarText: "GT" },
          { company: "Horizon Capital", action: "Compliance Flag Raised", time: "15 min ago", avatarText: "HC" },
          { company: "BlueSky Logistics", action: "Fee Invoice Sent", time: "22 min ago", avatarText: "BL" },
          { company: "Vertex Holdings", action: "Director KYC Approved", time: "1 hr ago", avatarText: "VH" },
        ],
        approvalQueue: [
          { company: "Quantum Freight FZE", type: "New License", submitted: "2 days ago", priority: "HIGH", id: "APP-90012" },
          { company: "Meridian Foods LLC", type: "Amendment", submitted: "1 day ago", priority: "MEDIUM", id: "APP-90033" },
          { company: "Delta Pharma Corp", type: "Renewal", submitted: "3 days ago", priority: "HIGH", id: "APP-90041" },
          { company: "Sunrise Media FZ-LLC", type: "Activity Change", submitted: "5 hrs ago", priority: "LOW", id: "APP-90052" },
          { company: "Atlas Energy Partners", type: "Director Change", submitted: "1 day ago", priority: "MEDIUM", id: "APP-90066" },
        ],
        zones: [
          { name: "Zone A", occupancy: 0.87, breakdown: { office: 48, warehouse: 34, land: 18 }, tenants: 62, utilities: { power: "green", water: "amber", internet: "green" } },
          { name: "Zone B", occupancy: 0.62, breakdown: { office: 44, warehouse: 22, land: 34 }, tenants: 39, utilities: { power: "green", water: "green", internet: "amber" } },
          { name: "Zone C", occupancy: 0.45, breakdown: { office: 28, warehouse: 26, land: 46 }, tenants: 21, utilities: { power: "amber", water: "amber", internet: "green" } },
          { name: "Tech Cluster", occupancy: 0.91, breakdown: { office: 60, warehouse: 12, land: 28 }, tenants: 74, utilities: { power: "amber", water: "green", internet: "green" } },
          { name: "Logistics Hub", occupancy: 0.78, breakdown: { office: 18, warehouse: 62, land: 20 }, tenants: 58, utilities: { power: "green", water: "amber", internet: "amber" } },
          { name: "Industrial Area", occupancy: 0.53, breakdown: { office: 22, warehouse: 34, land: 44 }, tenants: 33, utilities: { power: "amber", water: "green", internet: "red" } },
        ],
        businesses: [
          {
            id: "biz-1",
            regNo: "NZ-001204",
            companyName: "Al Masdar Trading LLC",
            tradeName: "Al Masdar Commodities",
            entityType: "FZ-LLC",
            industry: "Commodities",
            zone: "Zone A",
            licenseExp: "Dec 2025",
            status: "Active",
            shareholders: 6,
            regDate: "2022-06-19",
            activities: "Trading, Storage & Distribution",
            address: "Industrial District, Dockline Road",
            capital: "$3,500,000",
            visaUsed: 56,
            visaTotal: 80,
            bankMasked: "•••• 4491",
            contact: "licensing@nexuzzone.ae",
            documents: [
              { name: "Certificate of Incorporation", meta: "Uploaded 2022-06-22", badge: "Verified" },
              { name: "Trade License Renewal Proof", meta: "Uploaded 2025-12-03", badge: "Verified" },
            ],
            directors: ["A. Rahman", "S. Al Noor"],
            licenses: [
              { name: "NZ-L-441020", type: "Trading License", exp: "Dec 2025", status: "Active" },
              { name: "NZ-L-441887", type: "Warehouse Permit", exp: "Jun 2026", status: "Active" },
            ],
            invoices: [{ name: "INV-77112", service: "Annual Renewal", amount: "$3,000", status: "Paid" }],
            compliance: { status: "Compliant", kyc: "Verified", ubo: "Declared", lastAudit: "2026-02-18" },
            complianceFlags: [
              { reason: "License Activity Match", severity: "LOW", since: "—", assignedTo: "Compliance Desk", status: "Cleared" },
            ],
            audit: [
              { ts: "2026-02-18 11:14", action: "Renewed License", before: "Expiring", after: "Active", ip: "10.24.8.11" },
              { ts: "2025-12-03 09:42", action: "Document Verified", before: "Pending Verification", after: "Verified", ip: "10.24.8.11" },
            ],
          },
          {
            id: "biz-2",
            regNo: "NZ-001587",
            companyName: "GreenTech Innovations FZE",
            tradeName: "GreenTech Innovations",
            entityType: "FZE",
            industry: "Technology",
            zone: "Tech Cluster",
            licenseExp: "Mar 2026",
            status: "Active",
            shareholders: 9,
            regDate: "2023-03-02",
            activities: "SaaS & Green Infrastructure",
            address: "Tech Cluster, Building 7",
            capital: "$1,250,000",
            visaUsed: 44,
            visaTotal: 60,
            bankMasked: "•••• 1102",
            contact: "compliance@greentech.fz",
            documents: [
              { name: "Director KYC Pack", meta: "Uploaded 2024-12-21", badge: "Verified" },
              { name: "Visa Allocation Request", meta: "Uploaded 2026-01-09", badge: "Verified" },
            ],
            directors: ["M. Chen", "N. Noor"],
            licenses: [{ name: "NZ-L-551203", type: "Technology License", exp: "Mar 2026", status: "Active" }],
            invoices: [{ name: "INV-77401", service: "Visa Application", amount: "$4,250", status: "Paid" }],
            compliance: { status: "Compliant", kyc: "Verified", ubo: "Declared", lastAudit: "2026-01-28" },
            complianceFlags: [
              { reason: "Overdue Fees", severity: "AMBER", since: "2026-01-01", assignedTo: "Finance Officer", status: "Resolved" },
            ],
            audit: [
              { ts: "2026-01-28 15:03", action: "KYC Verified", before: "Pending", after: "Verified", ip: "10.24.8.44" },
            ],
          },
          {
            id: "biz-3",
            regNo: "NZ-001891",
            companyName: "Horizon Capital Offshore",
            tradeName: "Horizon Capital Offshore",
            entityType: "Offshore",
            industry: "Finance",
            zone: "Financial Services",
            licenseExp: "Jun 2025",
            status: "Suspended",
            shareholders: 3,
            regDate: "2020-10-09",
            activities: "Advisory & Wealth Management",
            address: "Harborfront Tower, Level 18",
            capital: "$5,900,000",
            visaUsed: 18,
            visaTotal: 40,
            bankMasked: "•••• 7780",
            contact: "aml@horizon-offshore.com",
            documents: [
              { name: "AML Declaration", meta: "Uploaded 2025-09-14", badge: "Verified" },
              { name: "Missing Board Minutes", meta: "Uploaded 2025-11-02", badge: "Pending" },
            ],
            directors: ["T. Berg", "R. Saeed"],
            licenses: [{ name: "NZ-L-331002", type: "Offshore License", exp: "Jun 2025", status: "Suspended" }],
            invoices: [{ name: "INV-76980", service: "Annual Renewal", amount: "$3,000", status: "Overdue" }],
            compliance: { status: "Restricted", kyc: "Expired", ubo: "Declared", lastAudit: "2025-12-07" },
            complianceFlags: [
              { reason: "AML Concern", severity: "HIGH", since: "2025-11-15", assignedTo: "Compliance Officer", status: "Open" },
              { reason: "Overdue Fees", severity: "AMBER", since: "2025-07-01", assignedTo: "Finance Desk", status: "Open" },
            ],
            audit: [{ ts: "2025-12-07 10:22", action: "Entity Suspended", before: "Active", after: "Suspended", ip: "10.24.9.5" }],
          },
          {
            id: "biz-4",
            regNo: "NZ-002043",
            companyName: "BlueSky Logistics Corp",
            tradeName: "BlueSky Logistics",
            entityType: "FZ-LLC",
            industry: "Logistics",
            zone: "Logistics Hub",
            licenseExp: "Aug 2026",
            status: "Active",
            shareholders: 7,
            regDate: "2021-01-24",
            activities: "Warehousing, Freight & Cross-dock",
            address: "Logistics Hub, Zone 3",
            capital: "$2,600,000",
            visaUsed: 62,
            visaTotal: 90,
            bankMasked: "•••• 5023",
            contact: "ops@bluesky-logistics.co",
            documents: [{ name: "Lease Agreement", meta: "Uploaded 2021-02-02", badge: "Verified" }],
            directors: ["K. Ibrahim", "L. Duarte"],
            licenses: [{ name: "NZ-L-882104", type: "Logistics License", exp: "Aug 2026", status: "Active" }],
            invoices: [{ name: "INV-78022", service: "Annual Renewal", amount: "$3,000", status: "Paid" }],
            compliance: { status: "Compliant", kyc: "Verified", ubo: "Declared", lastAudit: "2026-03-01" },
            complianceFlags: [],
            audit: [{ ts: "2026-03-01 08:55", action: "Lease Updated", before: "Old Term", after: "Current Term", ip: "10.24.8.11" }],
          },
          {
            id: "biz-5",
            regNo: "NZ-002187",
            companyName: "Vertex Holdings Ltd",
            tradeName: "Vertex Holdings",
            entityType: "FZE",
            industry: "Investment",
            zone: "Zone B",
            licenseExp: "Pending",
            status: "Pending",
            shareholders: 2,
            regDate: "2024-04-18",
            activities: "Investment Management & Advisory",
            address: "Financial Services Avenue",
            capital: "$900,000",
            visaUsed: 10,
            visaTotal: 25,
            bankMasked: "•••• 9012",
            contact: "growth@vertex-holdings.ai",
            documents: [{ name: "Initial License Pack", meta: "Uploaded 2026-03-05", badge: "Pending" }],
            directors: ["S. Rahman"],
            licenses: [],
            invoices: [{ name: "INV-78410", service: "Registration", amount: "$5,000", status: "Pending" }],
            compliance: { status: "Under Review", kyc: "Pending", ubo: "Pending", lastAudit: "—" },
            complianceFlags: [
              { reason: "Missing Documents", severity: "HIGH", since: "2026-03-05", assignedTo: "Compliance Desk", status: "Open" },
            ],
            audit: [{ ts: "2026-03-05 12:10", action: "Application Submitted", before: "—", after: "Pending", ip: "10.24.7.31" }],
          },
          {
            id: "biz-6",
            regNo: "NZ-002304",
            companyName: "Quantum Freight Partners",
            tradeName: "Quantum Freight Partners",
            entityType: "Branch",
            industry: "Logistics",
            zone: "Logistics Hub",
            licenseExp: "Oct 2025",
            status: "Processing",
            shareholders: 1,
            regDate: "2023-09-14",
            activities: "Freight & Logistics Services",
            address: "Logistics Hub, Warehouse Row B",
            capital: "$320,000",
            visaUsed: 8,
            visaTotal: 18,
            bankMasked: "•••• 6601",
            contact: "branch@qfp.example",
            documents: [
              { name: "Branch Registration Proof", meta: "Uploaded 2025-08-17", badge: "Verified" },
              { name: "Director Amendment", meta: "Uploaded 2025-10-05", badge: "Pending" },
            ],
            directors: ["H. Okafor"],
            licenses: [{ name: "NZ-L-99102", type: "Branch License", exp: "Oct 2025", status: "Processing" }],
            invoices: [{ name: "INV-77002", service: "Amendment Fee", amount: "$500", status: "Pending" }],
            compliance: { status: "In Review", kyc: "Verified", ubo: "Pending", lastAudit: "2025-10-14" },
            complianceFlags: [{ reason: "UBO Not Declared", severity: "AMBER", since: "2025-10-05", assignedTo: "Compliance Officer", status: "Open" }],
            audit: [{ ts: "2025-10-14 13:19", action: "Docs Pending", before: "Verified", after: "Pending", ip: "10.24.8.90" }],
          },
          {
            id: "biz-7",
            regNo: "NZ-002456",
            companyName: "Meridian Foods & Beverages",
            tradeName: "Meridian Foods",
            entityType: "FZ-LLC",
            industry: "F&B",
            zone: "Zone A",
            licenseExp: "Jan 2026",
            status: "Active",
            shareholders: 5,
            regDate: "2019-07-01",
            activities: "Food distribution & Import handling",
            address: "Zone A, Food Park 2",
            capital: "$1,800,000",
            visaUsed: 26,
            visaTotal: 55,
            bankMasked: "•••• 2440",
            contact: "licenses@meridian-fb.ae",
            documents: [{ name: "Health Compliance Certificate", meta: "Uploaded 2025-11-28", badge: "Verified" }],
            directors: ["Y. Ibrahim", "C. Mensah"],
            licenses: [{ name: "NZ-L-402110", type: "Food & Beverage License", exp: "Jan 2026", status: "Active" }],
            invoices: [{ name: "INV-77322", service: "Annual Renewal", amount: "$3,000", status: "Paid" }],
            compliance: { status: "Compliant", kyc: "Verified", ubo: "Declared", lastAudit: "2025-12-11" },
            complianceFlags: [],
            audit: [{ ts: "2025-12-11 09:10", action: "Annual Renewal Completed", before: "Due", after: "Active", ip: "10.24.8.11" }],
          },
          {
            id: "biz-8",
            regNo: "NZ-002598",
            companyName: "Delta Pharma International",
            tradeName: "Delta Pharma",
            entityType: "FZE",
            industry: "Healthcare",
            zone: "Zone C",
            licenseExp: "Nov 2025",
            status: "Active",
            shareholders: 4,
            regDate: "2022-11-30",
            activities: "Pharmaceutical import & distribution",
            address: "Healthcare Cluster, Lab Wing",
            capital: "$2,200,000",
            visaUsed: 33,
            visaTotal: 70,
            bankMasked: "•••• 8105",
            contact: "qa@delta-pharma.com",
            documents: [{ name: "Regulatory Approval Letter", meta: "Uploaded 2025-01-19", badge: "Verified" }],
            directors: ["S. Watanabe", "A. Al-Karim"],
            licenses: [{ name: "NZ-L-612900", type: "Healthcare License", exp: "Nov 2025", status: "Active" }],
            invoices: [{ name: "INV-77201", service: "Visa Application", amount: "$850", status: "Paid" }],
            compliance: { status: "Compliant", kyc: "Verified", ubo: "Declared", lastAudit: "2025-09-02" },
            complianceFlags: [{ reason: "Overdue Fees", severity: "LOW", since: "—", assignedTo: "Finance Officer", status: "Cleared" }],
            audit: [{ ts: "2025-09-02 16:04", action: "KYC Reviewed", before: "Valid", after: "Refreshed", ip: "10.24.9.10" }],
          },
          {
            id: "biz-9",
            regNo: "NZ-002701",
            companyName: "Sunrise Media FZ-LLC",
            tradeName: "Sunrise Media",
            entityType: "FZ-LLC",
            industry: "Media",
            zone: "Tech Cluster",
            licenseExp: "Apr 2026",
            status: "Active",
            shareholders: 3,
            regDate: "2021-08-09",
            activities: "Content studio & digital media production",
            address: "Tech Cluster, Studio 12",
            capital: "$480,000",
            visaUsed: 21,
            visaTotal: 40,
            bankMasked: "•••• 3311",
            contact: "ops@sunrise-media.fz",
            documents: [{ name: "Media Activity Declaration", meta: "Uploaded 2024-06-02", badge: "Verified" }],
            directors: ["P. Singh"],
            licenses: [{ name: "NZ-L-201991", type: "Media License", exp: "Apr 2026", status: "Active" }],
            invoices: [{ name: "INV-78600", service: "Visa Application", amount: "$2,550", status: "Paid" }],
            compliance: { status: "Compliant", kyc: "Verified", ubo: "Declared", lastAudit: "2026-02-20" },
            complianceFlags: [],
            audit: [{ ts: "2026-02-20 10:18", action: "Director KYC Approved", before: "Pending", after: "Verified", ip: "10.24.8.44" }],
          },
          {
            id: "biz-10",
            regNo: "NZ-002834",
            companyName: "Atlas Energy Partners",
            tradeName: "Atlas Energy Partners",
            entityType: "Offshore",
            industry: "Energy",
            zone: "Zone B",
            licenseExp: "Pending",
            status: "Processing",
            shareholders: 2,
            regDate: "2020-02-14",
            activities: "Energy trading & advisory",
            address: "Financial Services Avenue, Suite 44",
            capital: "$8,100,000",
            visaUsed: 28,
            visaTotal: 55,
            bankMasked: "•••• 7721",
            contact: "finance@atlas-energy.co",
            documents: [{ name: "Director Amendment Pack", meta: "Uploaded 2026-03-12", badge: "Pending" }],
            directors: ["D. Kovacs", "E. Noor"],
            licenses: [{ name: "NZ-L-888811", type: "Energy Offshore License", exp: "Pending", status: "Processing" }],
            invoices: [{ name: "INV-78777", service: "Activity Change", amount: "$500", status: "Pending" }],
            compliance: { status: "In Review", kyc: "Pending", ubo: "Declared", lastAudit: "—" },
            complianceFlags: [{ reason: "License Activity Mismatch", severity: "HIGH", since: "2026-03-12", assignedTo: "Compliance Officer", status: "Open" }],
            audit: [{ ts: "2026-03-12 09:05", action: "Docs Submitted", before: "—", after: "Processing", ip: "10.24.7.31" }],
          },
          {
            id: "biz-11",
            regNo: "NZ-002967",
            companyName: "NovaTech Solutions FZE",
            tradeName: "NovaTech Solutions",
            entityType: "FZE",
            industry: "IT Services",
            zone: "Tech Cluster",
            licenseExp: "Jul 2026",
            status: "Active",
            shareholders: 8,
            regDate: "2018-12-05",
            activities: "IT services & consultancy",
            address: "Tech Cluster, Office 4A",
            capital: "$1,100,000",
            visaUsed: 40,
            visaTotal: 80,
            bankMasked: "•••• 4401",
            contact: "legal@novatech.fz",
            documents: [{ name: "Annual Compliance Attestation", meta: "Uploaded 2026-01-18", badge: "Verified" }],
            directors: ["Z. Al-Hassan"],
            licenses: [{ name: "NZ-L-773101", type: "Technology License", exp: "Jul 2026", status: "Active" }],
            invoices: [{ name: "INV-77901", service: "Annual Renewal", amount: "$3,000", status: "Paid" }],
            compliance: { status: "Compliant", kyc: "Verified", ubo: "Declared", lastAudit: "2026-01-18" },
            complianceFlags: [],
            audit: [{ ts: "2026-01-18 12:41", action: "Renewal Confirmed", before: "Pending", after: "Active", ip: "10.24.8.11" }],
          },
          {
            id: "biz-12",
            regNo: "NZ-003012",
            companyName: "Pacific Rim Exports Ltd",
            tradeName: "Pacific Rim Exports",
            entityType: "Branch",
            industry: "Trading",
            zone: "Zone A",
            licenseExp: "Sep 2025",
            status: "Suspended",
            shareholders: 2,
            regDate: "2022-01-11",
            activities: "Export trading and customs interface",
            address: "Zone A, Trade Gate 1",
            capital: "$630,000",
            visaUsed: 12,
            visaTotal: 30,
            bankMasked: "•••• 3009",
            contact: "export@pacificrim.io",
            documents: [{ name: "License Violation Notice", meta: "Uploaded 2025-10-02", badge: "Verified" }],
            directors: ["G. Rivera"],
            licenses: [{ name: "NZ-L-123901", type: "Trading License", exp: "Sep 2025", status: "Suspended" }],
            invoices: [{ name: "INV-76111", service: "Late Renewal Penalty", amount: "$450", status: "Overdue" }],
            compliance: { status: "Restricted", kyc: "Expired", ubo: "Declared", lastAudit: "2025-10-12" },
            complianceFlags: [
              { reason: "Inactive Entity", severity: "AMBER", since: "2025-09-02", assignedTo: "Compliance Officer", status: "Open" },
            ],
            audit: [{ ts: "2025-10-12 13:00", action: "Entity Suspended", before: "Active", after: "Suspended", ip: "10.24.9.5" }],
          },
        ],
        applications: [
          { appId: "APP-90012", applicant: "Quantum Freight FZE", type: "New License", submitted: "2026-03-05", reviewer: "CK", slaDaysTotal: 10, slaDaysLeft: 8, status: "SUBMITTED", priority: "HIGH", companyId: "biz-6" },
          { appId: "APP-90033", applicant: "Meridian Foods LLC", type: "Amendment", submitted: "2026-03-04", reviewer: "VN", slaDaysTotal: 14, slaDaysLeft: 9, status: "UNDER REVIEW", priority: "MEDIUM", companyId: "biz-7" },
          { appId: "APP-90041", applicant: "Delta Pharma Corp", type: "Renewal", submitted: "2026-03-01", reviewer: "AR", slaDaysTotal: 12, slaDaysLeft: 6, status: "DOCS PENDING", priority: "HIGH", companyId: "biz-8" },
          { appId: "APP-90052", applicant: "Sunrise Media FZ-LLC", type: "Activity Change", submitted: "2026-02-28", reviewer: "SK", slaDaysTotal: 16, slaDaysLeft: 12, status: "UNDER REVIEW", priority: "LOW", companyId: "biz-9" },
          { appId: "APP-90066", applicant: "Atlas Energy Partners", type: "Director Change", submitted: "2026-03-02", reviewer: "RA", slaDaysTotal: 10, slaDaysLeft: 4, status: "DOCS PENDING", priority: "MEDIUM", companyId: "biz-10" },
          { appId: "APP-90077", applicant: "GreenTech FZE", type: "Visa Application", submitted: "2026-02-25", reviewer: "GT", slaDaysTotal: 8, slaDaysLeft: 5, status: "APPROVED", priority: "LOW", companyId: "biz-2" },
          { appId: "APP-90101", applicant: "Horizon Capital Offshore", type: "Renewal", submitted: "2026-02-20", reviewer: "HC", slaDaysTotal: 10, slaDaysLeft: 2, status: "REJECTED", priority: "HIGH", companyId: "biz-3" },
          { appId: "APP-90118", applicant: "Vertex Holdings", type: "Freezone Transfer", submitted: "2026-03-09", reviewer: "VH", slaDaysTotal: 20, slaDaysLeft: 16, status: "SUBMITTED", priority: "MEDIUM", companyId: "biz-5" },
          { appId: "APP-90133", applicant: "Pacific Rim Exports", type: "Deregistration", submitted: "2026-02-14", reviewer: "PR", slaDaysTotal: 18, slaDaysLeft: 3, status: "DOCS PENDING", priority: "HIGH", companyId: "biz-12" },
          { appId: "APP-90141", applicant: "BlueSky Logistics Corp", type: "Amendment", submitted: "2026-02-27", reviewer: "BL", slaDaysTotal: 14, slaDaysLeft: 11, status: "APPROVED", priority: "MEDIUM", companyId: "biz-4" },
          { appId: "APP-90156", applicant: "NovaTech Solutions FZE", type: "Activity Change", submitted: "2026-03-03", reviewer: "NS", slaDaysTotal: 16, slaDaysLeft: 7, status: "UNDER REVIEW", priority: "LOW", companyId: "biz-11" },
          { appId: "APP-90170", applicant: "BlueSky Logistics Corp", type: "Visa Application", submitted: "2026-03-10", reviewer: "BL", slaDaysTotal: 12, slaDaysLeft: 9, status: "SUBMITTED", priority: "MEDIUM", companyId: "biz-4" },
          { appId: "APP-90188", applicant: "Al Masdar Trading LLC", type: "Renewal", submitted: "2026-03-08", reviewer: "AR", slaDaysTotal: 10, slaDaysLeft: 6, status: "APPROVED", priority: "LOW", companyId: "biz-1" },
          { appId: "APP-90227", applicant: "GreenTech FZE", type: "Amendment", submitted: "2026-03-07", reviewer: "VN", slaDaysTotal: 14, slaDaysLeft: 1, status: "REJECTED", priority: "HIGH", companyId: "biz-2" },
          { appId: "APP-90239", applicant: "Quantum Freight Partners", type: "Director Change", submitted: "2026-03-06", reviewer: "CK", slaDaysTotal: 16, slaDaysLeft: 2, status: "REJECTED", priority: "MEDIUM", companyId: "biz-6" },
        ],
        licenses: [
          { id: "NZ-L-441020", companyId: "biz-1", company: "Al Masdar Trading LLC", type: "Trading License", activities: "Commodities Trading", issue: "Jan 2025", expiry: "Dec 2025", renewalStatus: "Renewal Due", status: "Active" },
          { id: "NZ-L-551203", companyId: "biz-2", company: "GreenTech Innovations FZE", type: "Technology License", activities: "SaaS Platform", issue: "Apr 2025", expiry: "Mar 2026", renewalStatus: "Renewed - Pending Settlement", status: "Active" },
          { id: "NZ-L-331002", companyId: "biz-3", company: "Horizon Capital Offshore", type: "Offshore License", activities: "Advisory", issue: "Feb 2024", expiry: "Jun 2025", renewalStatus: "Expired", status: "Suspended" },
          { id: "NZ-L-882104", companyId: "biz-4", company: "BlueSky Logistics Corp", type: "Logistics License", activities: "Warehousing & Freight", issue: "Sep 2024", expiry: "Aug 2026", renewalStatus: "Current", status: "Active" },
          { id: "NZ-L-201991", companyId: "biz-9", company: "Sunrise Media FZ-LLC", type: "Media License", activities: "Production", issue: "May 2025", expiry: "Apr 2026", renewalStatus: "Current", status: "Active" },
          { id: "NZ-L-612900", companyId: "biz-8", company: "Delta Pharma International", type: "Healthcare License", activities: "Pharmaceutical Distribution", issue: "Jan 2025", expiry: "Nov 2025", renewalStatus: "Expiring Soon", status: "Active" },
          { id: "NZ-L-402110", companyId: "biz-7", company: "Meridian Foods & Beverages", type: "Food & Beverage License", activities: "Food Distribution", issue: "Jan 2025", expiry: "Jan 2026", renewalStatus: "Current", status: "Active" },
          { id: "NZ-L-888811", companyId: "biz-10", company: "Atlas Energy Partners", type: "Energy Offshore License", activities: "Energy Trading", issue: "Nov 2025", expiry: "Pending", renewalStatus: "In Review", status: "Processing" },
          { id: "NZ-L-773101", companyId: "biz-11", company: "NovaTech Solutions FZE", type: "Technology License", activities: "IT Services", issue: "Oct 2024", expiry: "Jul 2026", renewalStatus: "Current", status: "Active" },
          { id: "NZ-L-99102", companyId: "biz-6", company: "Quantum Freight Partners", type: "Branch License", activities: "Freight & Logistics", issue: "Mar 2025", expiry: "Oct 2025", renewalStatus: "Processing", status: "Processing" },
          { id: "NZ-L-123901", companyId: "biz-12", company: "Pacific Rim Exports Ltd", type: "Trading License", activities: "Export Trading", issue: "Aug 2024", expiry: "Sep 2025", renewalStatus: "Expired", status: "Suspended" },
          { id: "NZ-L-551888", companyId: "biz-5", company: "Vertex Holdings Ltd", type: "Investment License", activities: "Advisory", issue: "Mar 2026", expiry: "Pending", renewalStatus: "Pending", status: "Pending" },
        ],
        investors: [
          {
            id: "inv-1",
            name: "Aisha Rahman",
            nationality: "🇦🇪",
            passportMasked: "••••4521",
            dob: "1989-07-14",
            visaStatus: "Active Visa",
            kycStatus: "Verified",
            kycBadge: "Verified ✅",
            associated: [{ companyId: "biz-1", company: "Al Masdar Trading LLC", role: "Director" }],
            documents: [
              { name: "Passport Copy", meta: "Uploaded 2025-11-02", verified: true },
              { name: "Emirates ID", meta: "Uploaded 2025-11-22", verified: true },
              { name: "Proof of Address", meta: "Uploaded 2025-12-10", verified: true },
            ],
            ubo: { status: "Declared", date: "2025-12-10" },
          },
          {
            id: "inv-2",
            name: "Mei Chen",
            nationality: "🇸🇬",
            passportMasked: "••••3307",
            dob: "1982-02-03",
            visaStatus: "Sponsored Visa",
            kycStatus: "Pending",
            kycBadge: "Pending 🟡",
            associated: [{ companyId: "biz-2", company: "GreenTech Innovations FZE", role: "Shareholder" }],
            documents: [
              { name: "Passport Copy", meta: "Uploaded 2026-01-09", verified: false },
              { name: "Emirates ID", meta: "Uploaded 2026-01-09", verified: false },
            ],
            ubo: { status: "Pending", date: "—" },
          },
          {
            id: "inv-3",
            name: "Tobias Berg",
            nationality: "🇩🇪",
            passportMasked: "••••1184",
            dob: "1976-09-28",
            visaStatus: "Expired Visa",
            kycStatus: "Expired",
            kycBadge: "Expired 🔴",
            associated: [{ companyId: "biz-3", company: "Horizon Capital Offshore", role: "Director" }],
            documents: [{ name: "Passport Copy", meta: "Uploaded 2024-09-12", verified: true }],
            ubo: { status: "Declared", date: "2025-09-14" },
          },
          {
            id: "inv-4",
            name: "Khadija Ibrahim",
            nationality: "🇦🇪",
            passportMasked: "••••9001",
            dob: "1991-04-19",
            visaStatus: "Active Visa",
            kycStatus: "Verified",
            kycBadge: "Verified ✅",
            associated: [{ companyId: "biz-4", company: "BlueSky Logistics Corp", role: "Manager" }],
            documents: [
              { name: "Passport Copy", meta: "Uploaded 2025-07-07", verified: true },
              { name: "Proof of Address", meta: "Uploaded 2025-08-18", verified: true },
            ],
            ubo: { status: "Declared", date: "2025-08-18" },
          },
          {
            id: "inv-5",
            name: "Sunil P. Singh",
            nationality: "🇮🇳",
            passportMasked: "••••4579",
            dob: "1986-12-02",
            visaStatus: "Active Visa",
            kycStatus: "Verified",
            kycBadge: "Verified ✅",
            associated: [{ companyId: "biz-9", company: "Sunrise Media FZ-LLC", role: "Director" }],
            documents: [{ name: "Emirates ID", meta: "Uploaded 2024-05-14", verified: true }],
            ubo: { status: "Declared", date: "2024-05-14" },
          },
          {
            id: "inv-6",
            name: "Aiko Al-Karim",
            nationality: "🇯🇵",
            passportMasked: "••••7710",
            dob: "1979-01-26",
            visaStatus: "Active Visa",
            kycStatus: "Pending",
            kycBadge: "Pending 🟡",
            associated: [{ companyId: "biz-8", company: "Delta Pharma International", role: "Shareholder" }],
            documents: [{ name: "Passport Copy", meta: "Uploaded 2026-02-18", verified: false }],
            ubo: { status: "Pending", date: "—" },
          },
          {
            id: "inv-7",
            name: "Zaid Al-Hassan",
            nationality: "🇸🇦",
            passportMasked: "••••2048",
            dob: "1984-06-09",
            visaStatus: "Active Visa",
            kycStatus: "Verified",
            kycBadge: "Verified ✅",
            associated: [{ companyId: "biz-11", company: "NovaTech Solutions FZE", role: "Director" }],
            documents: [{ name: "Proof of Address", meta: "Uploaded 2026-01-18", verified: true }],
            ubo: { status: "Declared", date: "2026-01-18" },
          },
          {
            id: "inv-8",
            name: "Rafael Saeed",
            nationality: "🇪🇬",
            passportMasked: "••••8803",
            dob: "1973-10-11",
            visaStatus: "Expired Visa",
            kycStatus: "Expired",
            kycBadge: "Expired 🔴",
            associated: [{ companyId: "biz-12", company: "Pacific Rim Exports Ltd", role: "Director" }],
            documents: [{ name: "Passport Copy", meta: "Uploaded 2024-03-28", verified: true }],
            ubo: { status: "Declared", date: "2024-03-28" },
          },
          {
            id: "inv-9",
            name: "Nora Noor",
            nationality: "🇨🇦",
            passportMasked: "••••9017",
            dob: "1993-08-30",
            visaStatus: "Sponsored Visa",
            kycStatus: "Verified",
            kycBadge: "Verified ✅",
            associated: [{ companyId: "biz-10", company: "Atlas Energy Partners", role: "Shareholder" }],
            documents: [{ name: "Emirates ID", meta: "Uploaded 2026-03-05", verified: true }],
            ubo: { status: "Declared", date: "2026-03-05" },
          },
        ],
        feeSchedule: [
          { service: "New License - FZE", entityType: "FZE", fee: 5000, vat: 0.05, total: 5250 },
          { service: "New License - FZ-LLC", entityType: "FZ-LLC", fee: 3500, vat: 0.05, total: 3675 },
          { service: "Annual Renewal - FZE", entityType: "FZE", fee: 3000, vat: 0.05, total: 3150 },
          { service: "Visa Application (per visa)", entityType: "—", fee: 850, vat: 0.05, total: 893.5 },
          { service: "Amendment Fee", entityType: "—", fee: 500, vat: 0.05, total: 525 },
          { service: "Late Renewal Penalty (per month)", entityType: "—", fee: 150, vat: 0.05, total: 157.5 },
        ],
        invoices: [
          { id: "INV-77112", company: "Al Masdar Trading LLC", service: "Annual Renewal", amount: "$3,000", due: "2026-03-10", status: "Paid ✅" },
          { id: "INV-78022", company: "BlueSky Logistics Corp", service: "Annual Renewal", amount: "$3,000", due: "2026-04-04", status: "Pending ⏳" },
          { id: "INV-76980", company: "Horizon Capital Offshore", service: "Annual Renewal", amount: "$3,000", due: "2026-01-15", status: "Overdue 🔴" },
          { id: "INV-77322", company: "Meridian Foods & Beverages", service: "Annual Renewal", amount: "$3,000", due: "2026-02-02", status: "Paid ✅" },
          { id: "INV-78777", company: "Atlas Energy Partners", service: "Activity Change", amount: "$500", due: "2026-03-21", status: "Pending ⏳" },
          { id: "INV-76111", company: "Pacific Rim Exports Ltd", service: "Late Renewal Penalty", amount: "$450", due: "2025-12-02", status: "Overdue 🔴" },
          { id: "INV-77401", company: "GreenTech Innovations FZE", service: "Visa Application", amount: "$4,250", due: "2026-03-18", status: "Disputed ⚠️" },
          { id: "INV-78600", company: "Sunrise Media FZ-LLC", service: "Visa Application", amount: "$2,550", due: "2026-03-30", status: "Pending ⏳" },
        ],
        flaggedCompanies: [
          { company: "Horizon Capital Offshore", reason: "AML Concern", severity: "HIGH", since: "2025-11-15", assigned: "Compliance Officer", status: "Open" },
          { company: "Vertex Holdings Ltd", reason: "Missing Documents", severity: "HIGH", since: "2026-03-05", assigned: "Compliance Desk", status: "Open" },
          { company: "Pacific Rim Exports Ltd", reason: "Inactive Entity", severity: "AMBER", since: "2025-09-02", assigned: "Compliance Officer", status: "Open" },
          { company: "Atlas Energy Partners", reason: "License Activity Mismatch", severity: "HIGH", since: "2026-03-12", assigned: "Compliance Officer", status: "Open" },
          { company: "BlueSky Logistics Corp", reason: "Overdue Fees", severity: "LOW", since: "2026-02-08", assigned: "Finance Desk", status: "Monitoring" },
          { company: "NovaTech Solutions FZE", reason: "UBO Not Declared", severity: "AMBER", since: "2026-02-14", assigned: "Compliance Officer", status: "Draft" },
        ],
        auditLog: [
          { ts: "2026-03-12 09:05", action: "Docs Submitted", entity: "Atlas Energy Partners", by: "NEX Admin", before: "—", after: "Processing", ip: "10.24.7.31" },
          { ts: "2026-03-07 13:48", action: "Fee Waiver Applied", entity: "Meridian Foods & Beverages", by: "Finance Officer", before: "$3,000", after: "$2,500", ip: "10.24.8.44" },
          { ts: "2026-03-03 16:22", action: "License Renewed", entity: "Al Masdar Trading LLC", by: "NEX Admin", before: "Expiring", after: "Active", ip: "10.24.8.11" },
          { ts: "2026-02-28 10:18", action: "Compliance Freeze Lifted", entity: "GreenTech Innovations FZE", by: "Compliance Officer", before: "Frozen", after: "Active", ip: "10.24.9.10" },
          { ts: "2026-02-20 07:33", action: "SAR Draft Generated", entity: "Horizon Capital Offshore", by: "Compliance Officer", before: "—", after: "SAR Drafted", ip: "10.24.9.5" },
          { ts: "2026-02-14 14:55", action: "KYC Status Updated", entity: "NovaTech Solutions FZE", by: "Reviewer", before: "Pending", after: "Verified", ip: "10.24.8.90" },
        ],
        users: [
          { user: "Admin (NEX)", role: "Super Admin", lastLogin: "2 min ago", twoFA: "Enabled", status: "Active", action: "Manage" },
          { user: "Zonal Ops", role: "Zone Admin", lastLogin: "18 min ago", twoFA: "Enabled", status: "Active", action: "Manage" },
          { user: "Reviewer Desk", role: "Reviewer", lastLogin: "5 hrs ago", twoFA: "Enabled", status: "Active", action: "Manage" },
          { user: "Finance Officer", role: "Finance Officer", lastLogin: "1 day ago", twoFA: "Enabled", status: "Active", action: "Manage" },
          { user: "Compliance Officer", role: "Compliance Officer", lastLogin: "3 days ago", twoFA: "Disabled", status: "Suspended", action: "Manage" },
          { user: "Read Only QA", role: "Read Only", lastLogin: "6 days ago", twoFA: "Enabled", status: "Active", action: "Manage" },
        ],
      };

      // ---------------------------
      // Helpers
      // ---------------------------
      const $ = (sel, root = document) => root.querySelector(sel);
      const $$ = (sel, root = document) => Array.from(root.querySelectorAll(sel));

      function formatMoney(n) {
        if (typeof n !== "number") return String(n);
        return n.toLocaleString(undefined, { maximumFractionDigits: 0 });
      }

      function showToast(title, subtitle = "", kind = "gold") {
        const container = $("#toastContainer");
        const toast = document.createElement("div");
        toast.className = "toast";
        const accent =
          kind === "danger"
            ? "rgba(227,93,106,1)"
            : kind === "blue"
              ? "rgba(77,163,255,1)"
              : "var(--gold)";
        toast.innerHTML = `
          <div class="t" style="color:${accent};font-weight:800;">${title}</div>
          ${subtitle ? `<div class="s">${subtitle}</div>` : ""}
        `;
        container.appendChild(toast);
        const t = setTimeout(() => {
          toast.style.opacity = "0";
          toast.style.transform = "translateY(10px)";
          toast.style.transition = "opacity 200ms ease, transform 200ms ease";
        }, 2600);
        const t2 = setTimeout(() => {
          toast.remove();
          clearTimeout(t);
          clearTimeout(t2);
        }, 2900);
      }

      function setTheme(isLight) {
        document.body.classList.toggle("light", isLight);
        try {
          localStorage.setItem("nexus-theme", isLight ? "light" : "dark");
        } catch {
          // ignore
        }
      }

      function statusPill(status) {
        const cls = statusToPillClass[status] || "";
        return `<span class="pill ${cls}">${status}</span>`;
      }

      function priorityBadge(priority) {
        const p = String(priority).toUpperCase();
        const cls = p === "HIGH" ? "high" : p === "MEDIUM" ? "medium" : "low";
        return `<span class="priority ${cls}">${p}</span>`;
      }

      function clamp(v, min, max) {
        return Math.max(min, Math.min(max, v));
      }

      function extractYearFromLicenseExp(licenseExp) {
        const s = String(licenseExp || "");
        const m = s.match(/(19|20)\d{2}/);
        return m ? m[0] : "";
      }

      function findBusinessIdByCompanyName(name) {
        const q = String(name || "").trim().toLowerCase();
        if (!q) return null;
        const biz = mock.businesses.find((b) => {
          const cn = String(b.companyName || "").toLowerCase();
          const tn = String(b.tradeName || "").toLowerCase();
          const rn = String(b.regNo || "").toLowerCase();
          return cn === q || tn === q || rn === q || cn.includes(q) || tn.includes(q) || q.includes(cn) || q.includes(tn);
        });
        return biz ? biz.id : null;
      }

      function getOccupancyColor(occ) {
        if (occ < 0.5) return "green";
        if (occ < 0.8) return "amber";
        return "red";
      }

      function occupancyFill(occ) {
        if (occ < 0.5) return "rgba(47,227,139,0.24)";
        if (occ < 0.8) return "rgba(242,184,75,0.22)";
        return "rgba(227,93,106,0.22)";
      }

      // ---------------------------
      // Navigation + view switching
      // ---------------------------
      const viewButtons = $$(".nav-btn");
      const views = $$("section.view");
      const viewTitleByKey = {
        dashboard: "Dashboard",
        registry: "Business Registry",
        applications: "Applications & Approvals",
        licenses: "License Management",
        investors: "Investor Profiles",
        infrastructure: "Zone Infrastructure",
        fees: "Fees & Invoicing",
        compliance: "Compliance & Audits",
        analytics: "Analytics & Reports",
        settings: "System Settings",
        portal: "Portal Configuration",
        permissions: "Access & Permissions",
      };

      function setActiveView(key) {
        views.forEach((s) => {
          s.classList.toggle("active", s.dataset.view === key);
        });
        viewButtons.forEach((b) => b.classList.toggle("active", b.dataset.view === key));

        const title = viewTitleByKey[key] || "NEXUS ZONE";
        $("#topbarTitle").textContent =
          key === "dashboard" ? "Welcome back, Administrator" : title;

        document.documentElement.scrollTop = 0;
        fadeStaggerOnView();
      }

      function fadeStaggerOnView() {
        // Re-trigger stagger effects for the active view
        $$(".fade-stagger").forEach((el) => el.classList.remove("visible"));
        const active = $("section.view.active");
        if (!active) return;
        const items = $$(":scope .fade-stagger", active);
        items.forEach((el, idx) => {
          const d = Math.min(420, idx * 45);
          el.style.transitionDelay = `${d}ms`;
          el.classList.add("visible");
        });
      }

      viewButtons.forEach((btn) => {
        btn.addEventListener("click", () => {
          setActiveView(btn.dataset.view);
          closeMobileSidebar();
        });
      });

      // ---------------------------
      // Sidebar: mobile + collapse
      // ---------------------------
      const sidebar = $("#sidebar");
      const sidebarBackdrop = $("#sidebarBackdrop");
      const mobileMenuBtn = $("#mobileMenuBtn");
      const mobileGodBtn = $("#mobileGodBtn");
      const godFloat = $("#godFloat");

      function openMobileSidebar() {
        sidebar.classList.add("open");
        sidebarBackdrop.classList.add("show");
      }
      function closeMobileSidebar() {
        sidebar.classList.remove("open");
        sidebarBackdrop.classList.remove("show");
      }

      mobileMenuBtn?.addEventListener("click", () => {
        if (sidebar.classList.contains("open")) closeMobileSidebar();
        else openMobileSidebar();
      });
      sidebarBackdrop?.addEventListener("click", closeMobileSidebar);

      // ---------------------------
      // Theme toggle + date
      // ---------------------------
      const darkToggle = $("#darkToggle");
      function initTheme() {
        const stored = (() => {
          try {
            return localStorage.getItem("nexus-theme");
          } catch {
            return null;
          }
        })();
        setTheme(stored === "light");
      }
      initTheme();

      darkToggle?.addEventListener("click", () => {
        setTheme(!document.body.classList.contains("light"));
        showToast("Theme switched", "Dark/Light mode toggled", "blue");
      });

      function setNowDate() {
        const d = new Date();
        $("#topbarDate").textContent = d.toLocaleDateString(undefined, {
          weekday: "short",
          year: "numeric",
          month: "short",
          day: "2-digit",
        });
      }
      setNowDate();
      setInterval(setNowDate, 60 * 1000);

      // ---------------------------
      // Notifications
      // ---------------------------
      const notifBtn = $("#notifBtn");
      const notifPanel = $("#notifPanel");
      const notifList = $("#notifList");

      const notifications = [
        {
          id: "n1",
          color: "red",
          title: "Compliance Alert",
          body: "AML concern raised for Horizon Capital Offshore.",
          pill: "Red",
        },
        {
          id: "n2",
          color: "amber",
          title: "Expiry Warning",
          body: "6 licenses nearing renewal window in Zone A.",
          pill: "Amber",
        },
        {
          id: "n3",
          color: "blue",
          title: "New Application",
          body: "Vertex Holdings submitted a freezone transfer request.",
          pill: "Blue",
        },
      ];

      function buildNotifications() {
        notifList.innerHTML = "";
        notifications.forEach((n) => {
          const item = document.createElement("div");
          item.className = "notif-item";
          const dot =
            n.color === "red"
              ? "background:rgba(227,93,106,0.95)"
              : n.color === "amber"
                ? "background:rgba(242,184,75,0.95)"
                : "background:rgba(77,163,255,0.95)";
          item.innerHTML = `
            <div class="notif-dot" style="${dot}"></div>
            <div class="txt">
              <div class="a">${n.title}</div>
              <div class="b">${n.body}</div>
            </div>
          `;
          item.addEventListener("click", () => {
            notifPanel.classList.remove("show");
            showToast(n.title, n.body, n.color === "red" ? "danger" : "gold");
          });
          notifList.appendChild(item);
        });
      }
      buildNotifications();

      function toggleNotifPanel() {
        const expanded = notifPanel.classList.toggle("show");
        notifBtn?.setAttribute("aria-expanded", String(expanded));
      }
      notifBtn?.addEventListener("click", (e) => {
        e.stopPropagation();
        toggleNotifPanel();
      });
      document.addEventListener("click", (e) => {
        if (!notifPanel.contains(e.target) && e.target !== notifBtn) notifPanel.classList.remove("show");
      });

      // ---------------------------
      // Floating God Mode badge + overlay
      // ---------------------------
      const godOverlayBackdrop = $("#godOverlayBackdrop");
      const godOverlayClose = $("#godOverlayClose");
      let godOverlayOpen = false;

      function openGodOverlay() {
        godOverlayBackdrop.classList.add("show");
        godOverlayBackdrop.setAttribute("aria-hidden", "false");
        godOverlayOpen = true;
      }
      function closeGodOverlay() {
        godOverlayBackdrop.classList.remove("show");
        godOverlayBackdrop.setAttribute("aria-hidden", "true");
        godOverlayOpen = false;
      }

      godFloat?.addEventListener("click", () => {
        godOverlayOpen ? closeGodOverlay() : openGodOverlay();
      });
      godOverlayClose?.addEventListener("click", closeGodOverlay);
      godOverlayBackdrop?.addEventListener("click", (e) => {
        if (e.target === godOverlayBackdrop) closeGodOverlay();
      });
      mobileGodBtn?.addEventListener("click", () => {
        godOverlayOpen ? closeGodOverlay() : openGodOverlay();
      });

      document.addEventListener("keydown", (e) => {
        const tag = (e.target && e.target.tagName) ? e.target.tagName.toLowerCase() : "";
        const isTyping = tag === "input" || tag === "textarea" || e.target?.isContentEditable;
        if (isTyping) return;
        if (e.key === "g" || e.key === "G") {
          e.preventDefault();
          setActiveView("permissions");
          godOverlayOpen ? closeGodOverlay() : openGodOverlay();
        }
        if (e.key === "Escape") {
          closeGodOverlay();
          closeDrawer();
          closeInvestorModal();
          closePinModal();
          closeContextMenu();
        }
      });

      // ---------------------------
      // PIN modal flow (4-digit 1234)
      // ---------------------------
      const pinBackdrop = $("#pinBackdrop");
      const pinCloseBtn = $("#pinCloseBtn");
      const pinHidden = $("#pinHidden");
      const pinDigs = [$("#pinD1"), $("#pinD2"), $("#pinD3"), $("#pinD4")];
      const pinPrompt = $("#pinPrompt");
      let pinResolve = null;
      let pinReject = null;
      let pinWrongOnce = false;

      function openPinModal({ actionName, promptText }) {
        pinWrongOnce = false;
        pinPrompt.textContent = promptText || `Confirm with PIN to proceed: ${actionName}`;
        pinBackdrop.classList.add("show");
        pinBackdrop.setAttribute("aria-hidden", "false");
        pinHidden.value = "";
        pinHidden.focus();
        pinDigs.forEach((d) => {
          d.textContent = "";
          d.classList.remove("error");
        });
      }

      function closePinModal() {
        pinBackdrop.classList.remove("show");
        pinBackdrop.setAttribute("aria-hidden", "true");
        pinResolve = null;
        pinReject = null;
      }

      pinCloseBtn?.addEventListener("click", () => {
        if (pinReject) pinReject(new Error("PIN cancelled"));
        closePinModal();
      });
      document.addEventListener("click", (e) => {
        if (e.target === pinBackdrop) {
          if (pinReject) pinReject(new Error("PIN cancelled"));
          closePinModal();
        }
      });

      function updatePinDigits(val) {
        const s = String(val);
        pinDigs.forEach((d, i) => {
          const ch = s[i] || " ";
          d.textContent = ch === " " ? "" : ch;
          d.classList.toggle("active", i === s.length - 1);
          d.classList.remove("error");
        });
      }

      pinHidden?.addEventListener("input", (e) => {
        const raw = e.target.value.replace(/\D+/g, "").slice(0, 4);
        e.target.value = raw;
        updatePinDigits(raw);

        if (raw.length === 4) {
          if (raw === "1234") {
            if (pinResolve) pinResolve();
            closePinModal();
            return;
          }
          if (!pinWrongOnce) {
            pinWrongOnce = true;
          }
          pinDigs.forEach((d) => d.classList.add("error"));
          pinPrompt.textContent = "Incorrect PIN. Try again.";
          updatePinDigits("");
          pinHidden.value = "";
        }
      });

      // Promise wrapper
      function requirePin(actionName, promptText) {
        return new Promise((resolve, reject) => {
          pinResolve = resolve;
          pinReject = reject;
          openPinModal({ actionName, promptText });
        });
      }

      // ---------------------------
      // Context menu: right-click on table rows
      // ---------------------------
      const contextMenu = $("#contextMenu");
      let ctxTarget = null;

      function openContextMenu(x, y, ctx) {
        ctxTarget = ctx;
        const maxX = window.innerWidth - contextMenu.offsetWidth - 10;
        const maxY = window.innerHeight - contextMenu.offsetHeight - 10;
        contextMenu.style.left = `${clamp(x, 10, maxX)}px`;
        contextMenu.style.top = `${clamp(y, 10, maxY)}px`;
        contextMenu.classList.add("show");
      }

      function closeContextMenu() {
        ctxTarget = null;
        contextMenu.classList.remove("show");
      }

      window.addEventListener("click", () => closeContextMenu());
      window.addEventListener("scroll", () => closeContextMenu());

      $$("[data-ctx]").forEach((item) => {
        item.addEventListener("click", async (e) => {
          e.stopPropagation();
          const ctxAction = item.dataset.ctx;
          const ctx = ctxTarget;
          closeContextMenu();
          if (!ctx) return;

          // Resolve companyId from companyName when context came from other tables.
          if (!ctx.companyId && ctx.companyName) {
            ctx.companyId = findBusinessIdByCompanyName(ctx.companyName);
          }

          const needsCompany = ["view", "suspend", "audit", "delete"].includes(ctxAction);
          if (needsCompany && !ctx.companyId) {
            showToast("Not supported", "This row is not linked to a company in mock data.", "gold");
            return;
          }

          if (ctxAction === "view") {
            openBusinessDrawer(ctx.companyId);
            return;
          }
          if (ctxAction === "suspend") {
            try {
              const ok = window.confirm(`Suspend entity: ${ctx.companyName}?`);
              if (!ok) return;
              await requirePin("Suspend", "Suspending requires PIN confirmation.");
              updateBusinessStatus(ctx.companyId, "Suspended");
              showToast("Suspended", ctx.companyName, "danger");
            } catch {
              showToast("Cancelled", "PIN confirmation cancelled.", "gold");
            }
            return;
          }
          if (ctxAction === "audit") {
            openBusinessDrawer(ctx.companyId, "audit");
            showToast("Audit Trail", `Opened audit log for ${ctx.companyName}`, "blue");
            return;
          }
          if (ctxAction === "export") {
            showToast("Downloading...", `Exporting profile for ${ctx.companyName}`, "blue");
            setTimeout(() => showToast("Export ready", "Simulated download completed."), 900);
            return;
          }
          if (ctxAction === "delete") {
            try {
              const ok = window.confirm(`Delete entity: ${ctx.companyName}? This is irreversible (mock).`);
              if (!ok) return;
              await requirePin("Delete", "Irreversible delete requires PIN.");
              // mock deletion: set status deregistered
              updateBusinessStatus(ctx.companyId, "Deregistered");
              showToast("Deleted (mock)", ctx.companyName, "danger");
            } catch {
              showToast("Cancelled", "PIN confirmation cancelled.", "gold");
            }
            return;
          }
          if (ctxAction === "edit") {
            showToast("Edit (mock)", `Opened editor for ${ctx.companyName}`, "blue");
          }
        });
      });

      // ---------------------------
      // Drawer: Business profile
      // ---------------------------
      const drawerBackdrop = $("#drawerBackdrop");
      const drawerCloseBtn = $("#drawerCloseBtn");
      const businessDrawer = $("#businessDrawer");
      const drawerTabs = $$(".tab", businessDrawer);

      function openDrawer() {
        drawerBackdrop.classList.add("show");
        businessDrawer.classList.add("open");
      }
      function closeDrawer() {
        drawerBackdrop.classList.remove("show");
        businessDrawer.classList.remove("open");
      }
      drawerCloseBtn?.addEventListener("click", closeDrawer);
      drawerBackdrop?.addEventListener("click", (e) => {
        if (e.target === drawerBackdrop) closeDrawer();
      });

      // (Fix: use correct tabs with data-tab)
      function setDrawerTab(tabKey) {
        $$(".tab", businessDrawer).forEach((btn) => btn.classList.toggle("active", (btn.dataset.tab || btn.dataset.itab) === tabKey));
        const map = {
          overview: "drawerOverview",
          documents: "drawerDocuments",
          directors: "drawerDirectors",
          licenses: "drawerLicenses",
          invoices: "drawerInvoices",
          compliance: "drawerCompliance",
          audit: "drawerAudit",
        };
        const targets = $$(".drawer-section", businessDrawer);
        targets.forEach((sec) => sec.classList.toggle("active", sec.id === map[tabKey]));
      }

      $$(`.tabs .tab`, businessDrawer).forEach((btn) => {
        btn.addEventListener("click", () => setDrawerTab(btn.dataset.tab));
      });

      let currentBusinessId = null;

      function updateDrawerWithBusiness(biz, tab = "overview") {
        currentBusinessId = biz.id;
        $("#drawerCompanyName").textContent = biz.companyName;
        $("#drawerRegBadge").textContent = `Reg# ${biz.regNo}`;

        const pillClass = statusToPillClass[biz.status] || "";
        const statusPillEl = $("#drawerStatusPill");
        statusPillEl.className = `pill ${pillClass}`; // reset
        statusPillEl.textContent = biz.status;

        $("#drawerRegDate").textContent = new Date(biz.regDate).toLocaleDateString(undefined, { year: "numeric", month: "short", day: "2-digit" });
        $("#drawerActivities").textContent = biz.activities;
        $("#drawerAddress").textContent = biz.address;
        $("#drawerCapital").textContent = biz.capital;
        $("#drawerVisaQuota").textContent = `${biz.visaUsed} used / ${biz.visaTotal} total`;
        $("#drawerBank").textContent = biz.bankMasked;
        $("#drawerContact").textContent = biz.contact;
        $("#drawerTradeName").textContent = biz.tradeName;

        $("#drawerDocsList").innerHTML = "";
        biz.documents.forEach((d) => {
          const el = document.createElement("div");
          el.className = "doc-item";
          el.innerHTML = `
            <div class="left">
              <div class="a" title="${d.name}">${d.name}</div>
              <div class="b">${d.meta}</div>
            </div>
            <div class="pill ${d.badge === "Verified" ? "active" : "pending"}" style="font-weight:800;">
              ${d.badge}
            </div>
          `;
          $("#drawerDocsList").appendChild(el);
        });

        $("#drawerDirectorsList").innerHTML = "";
        biz.directors.forEach((name) => {
          const el = document.createElement("div");
          el.className = "doc-item";
          el.innerHTML = `
            <div class="left">
              <div class="a" title="${name}">${name}</div>
              <div class="b">Director profile (mock)</div>
            </div>
            <button class="btn small ghost" type="button" data-requires-pin="false" data-action="View Director" title="Mock action">View</button>
          `;
          $("#drawerDirectorsList").appendChild(el);
        });

        $("#drawerLicensesList").innerHTML = "";
        if (biz.licenses.length === 0) {
          $("#drawerLicensesList").innerHTML = `<div style="color:var(--muted2);font-size:12px;padding:10px 0;">No licenses linked (mock).</div>`;
        } else {
          biz.licenses.forEach((l) => {
            const el = document.createElement("div");
            el.className = "doc-item";
            el.innerHTML = `
              <div class="left">
                <div class="a" title="${l.name}">${l.name}</div>
                <div class="b">${l.type} • Expires: ${l.exp}</div>
              </div>
              ${statusPill(l.status === "Processing" ? "Processing" : l.status === "Pending" ? "Pending" : l.status)}
            `;
            $("#drawerLicensesList").appendChild(el);
          });
        }

        $("#drawerInvoicesList").innerHTML = "";
        biz.invoices.forEach((inv) => {
          const el = document.createElement("div");
          el.className = "doc-item";
          const status = inv.status;
          let pillCls = "pending";
          if (status.includes("Paid")) pillCls = "active";
          else if (status.includes("Overdue")) pillCls = "suspended";
          else if (status.includes("Disputed")) pillCls = "processing";
          else if (status.includes("Waived")) pillCls = "active";
          el.innerHTML = `
            <div class="left">
              <div class="a" title="${inv.name}">${inv.name}</div>
              <div class="b">${inv.service} • Amount: ${inv.amount}</div>
            </div>
            <span class="pill ${pillCls}">${inv.status}</span>
          `;
          $("#drawerInvoicesList").appendChild(el);
        });

        $("#drawerComplianceStatus").textContent = biz.compliance.status;
        $("#drawerKyc").textContent = biz.compliance.kyc;
        $("#drawerUbo").textContent = biz.compliance.ubo;
        $("#drawerLastAudit").textContent = biz.compliance.lastAudit;

        $("#drawerComplianceFlags").innerHTML = "";
        if (biz.complianceFlags && biz.complianceFlags.length) {
          biz.complianceFlags.forEach((f) => {
            const el = document.createElement("div");
            el.className = "doc-item";
            const sevCls = f.severity === "HIGH" ? "suspended" : f.severity === "AMBER" ? "pending" : "processing";
            el.innerHTML = `
              <div class="left">
                <div class="a" title="${f.reason}">${f.reason}</div>
                <div class="b">Since: ${f.since} • Assigned: ${f.assignedTo || f.assigned || "—"}</div>
              </div>
              <span class="pill ${sevCls}">${f.severity || "—"}</span>
            `;
            $("#drawerComplianceFlags").appendChild(el);
          });
        } else {
          $("#drawerComplianceFlags").innerHTML = `<div style="color:var(--muted2);font-size:12px;padding:10px 0;">No active compliance flags.</div>`;
        }

        // Audit
        $("#drawerAuditTbody").innerHTML = "";
        biz.audit.forEach((a) => {
          const tr = document.createElement("tr");
          tr.innerHTML = `
            <td>${a.ts}</td>
            <td>${a.action}</td>
            <td>${a.before}</td>
            <td>${a.after}</td>
            <td>${a.ip}</td>
          `;
          $("#drawerAuditTbody").appendChild(tr);
        });

        setDrawerTab(tab);
      }

      function openBusinessDrawer(companyId, tab = "overview") {
        const biz = mock.businesses.find((b) => b.id === companyId) || mock.businesses[0];
        updateDrawerWithBusiness(biz, tab);
        openDrawer();
      }

      $("#drawerCompanyName")?.addEventListener("click", () => {});

      function updateBusinessStatus(companyId, newStatus) {
        const biz = mock.businesses.find((b) => b.id === companyId);
        if (!biz) return;
        biz.status = newStatus;

        // Mirror business control state into linked mock licenses
        mock.licenses.forEach((lic) => {
          if (lic.companyId !== companyId) return;
          if (newStatus === "Active") {
            lic.status = "Active";
            lic.renewalStatus = "Current";
          } else if (newStatus === "Suspended" || newStatus === "Processing") {
            lic.status = newStatus === "Processing" ? "Processing" : "Suspended";
            lic.renewalStatus = newStatus === "Processing" ? "Processing" : "Suspended";
          } else if (newStatus === "Deregistered") {
            lic.status = "Suspended";
            lic.renewalStatus = "Expired";
          } else if (newStatus === "Pending") {
            lic.status = "Pending";
            lic.renewalStatus = "Pending";
          }
        });

        // Refresh registry table row + drawer pill (if open)
        renderRegistryTable();
        if (currentBusinessId === companyId) {
          const updated = mock.businesses.find((b) => b.id === companyId);
          updateDrawerWithBusiness(updated, "overview");
        }
      }

      // Drawer quick action buttons
      $$("[data-action]", businessDrawer).forEach((btn) => {
        btn.addEventListener("click", async () => {
          const action = btn.dataset.action || "Action";
          const requiresPin = btn.dataset.requiresPin === "true";
          if (requiresPin) {
            const ok = window.confirm(`Proceed with: ${action}?`);
            if (!ok) return;
            try {
              await requirePin(action, `PIN required for: ${action}`);
            } catch {
              showToast("Cancelled", "PIN confirmation cancelled.", "gold");
              return;
            }
          }

          if (action === "Suspend") {
            updateBusinessStatus(currentBusinessId, "Suspended");
            showToast("Suspended", "Entity status updated (mock).", "danger");
          } else if (action === "Delete") {
            updateBusinessStatus(currentBusinessId, "Deregistered");
            showToast("Deleted (mock)", "Entity deregistered.", "danger");
          } else if (action === "Approve") {
            updateBusinessStatus(currentBusinessId, "Active");
            showToast("Approved", "Entity approved (mock).", "gold");
          } else if (action === "Renew License") {
            showToast("Renewal started", "Renewal process queued (mock).", "blue");
          } else if (action === "Export Profile") {
            showToast("Downloading...", `Exporting ${$("#drawerCompanyName").textContent} profile`, "blue");
          } else if (action === "Flag for Audit") {
            showToast("Flagged", "Compliance review queued (mock).", "gold");
          } else {
            showToast(action, "Executed (mock).", "blue");
          }
        });
      });

      // ---------------------------
      // Investor modal
      // ---------------------------
      const investorModalBackdrop = $("#investorModalBackdrop");
      const investorModal = $("#investorModal");
      const investorModalClose = $("#investorModalClose");
      const investorTabs = $$(".tabs .tab", investorModal);
      let investorCurrentId = null;

      function openInvestorModal(id) {
        const inv = mock.investors.find((x) => x.id === id);
        if (!inv) return;
        investorCurrentId = id;
        $("#investorModalName").textContent = `${inv.name} ${inv.nationality}`;
        $("#investorModalKycSummary").textContent = `KYC: ${inv.kycBadge} • Associated companies: ${inv.associated.length}`;

        $("#investorDOB").textContent = new Date(inv.dob).toLocaleDateString(undefined, { year: "numeric", month: "short", day: "2-digit" });
        $("#investorNationality").textContent = inv.nationality;
        $("#investorPassport").textContent = inv.passportMasked;
        $("#investorVisaStatus").textContent = inv.visaStatus;
        $("#investorUboStatus").textContent = inv.ubo.status;
        $("#investorUboDate").textContent = inv.ubo.date;

        $("#investorEntitiesTbody").innerHTML = "";
        inv.associated.forEach((a) => {
          const tr = document.createElement("tr");
          tr.innerHTML = `<td>${a.company}</td><td>${a.role}</td>`;
          $("#investorEntitiesTbody").appendChild(tr);
        });

        $("#investorDocsList").innerHTML = "";
        inv.documents.forEach((d) => {
          const el = document.createElement("div");
          el.className = "doc-item";
          el.innerHTML = `
            <div class="left">
              <div class="a" title="${d.name}">${d.name}</div>
              <div class="b">${d.meta}</div>
            </div>
            <span class="pill ${d.verified ? "active" : "pending"}">${d.verified ? "Verified" : "Pending"}</span>
          `;
          $("#investorDocsList").appendChild(el);
        });

        // default tab
        investorTabs.forEach((t) => t.classList.toggle("active", t.dataset.itab === "personal"));
        setInvestorTab("personal");

        investorModalBackdrop.classList.add("show");
        investorModalBackdrop.setAttribute("aria-hidden", "false");
      }

      function setInvestorTab(tabKey) {
        investorTabs.forEach((btn) => btn.classList.toggle("active", btn.dataset.itab === tabKey));
        const sections = $$(".drawer-section", investorModal);
        sections.forEach((sec) => {
          const active =
            (tabKey === "personal" && sec.id === "investorTabPersonal") ||
            (tabKey === "entities" && sec.id === "investorTabEntities") ||
            (tabKey === "documents" && sec.id === "investorTabDocuments") ||
            (tabKey === "ubo" && sec.id === "investorTabUbo");
          sec.classList.toggle("active", active);
        });
      }

      investorTabs.forEach((btn) => {
        btn.addEventListener("click", () => setInvestorTab(btn.dataset.itab));
      });

      function closeInvestorModal() {
        investorModalBackdrop.classList.remove("show");
        investorModalBackdrop.setAttribute("aria-hidden", "true");
        investorCurrentId = null;
      }
      investorModalClose?.addEventListener("click", closeInvestorModal);
      investorModalBackdrop?.addEventListener("click", (e) => {
        if (e.target === investorModalBackdrop) closeInvestorModal();
      });

      // Investor non-PIN actions (PIN actions are handled by the global PIN binder).
      $$("[data-action]", investorModal)
        .filter((btn) => btn.dataset.requiresPin !== "true")
        .forEach((btn) => {
          btn.addEventListener("click", () => {
            const actionName = btn.dataset.action || btn.textContent.trim();
            showToast("Executed (mock)", actionName, "gold");
          });
        });

      // ---------------------------
      // Rendering: Dashboard
      // ---------------------------
      function buildActivityFeed() {
        const feed = $("#activityFeed");
        feed.innerHTML = "";
        mock.activityFeed.forEach((a) => {
          const item = document.createElement("div");
          item.className = "feed-item";
          item.setAttribute("role", "listitem");
          item.innerHTML = `
            <div class="avatar-mini" aria-hidden="true">${a.avatarText || "NZ"}</div>
            <div class="t">
              <div class="a" title="${a.company} — ${a.action}">
                <span style="color:var(--text);font-weight:800;">${a.company}</span> — ${a.action}
              </div>
              <div class="b">${a.time}</div>
            </div>
          `;
          feed.appendChild(item);
        });
      }

      buildActivityFeed();

      function renderApprovalQueue() {
        const tbody = $("#approvalQueue");
        tbody.innerHTML = "";
        mock.approvalQueue?.forEach?.(() => {}); // not present; use mock.approvalQueue-like data; queue uses from activity feed
        const top5 = mock.approvalQueue || [
          { company: "Quantum Freight FZE", type: "New License", submitted: "2 days ago", priority: "HIGH", id: "APP-90012" },
        ];
      }

      // We'll build queue using applications with status "DOCS PENDING" or "UNDER REVIEW"
      function buildDashboardApprovalQueue() {
        const tbody = $("#approvalQueue");
        tbody.innerHTML = "";
        const queue = mock.applications
          .filter((a) => ["SUBMITTED", "UNDER REVIEW", "DOCS PENDING"].includes(a.status))
          .slice(0, 5);

        queue.forEach((app) => {
          const tr = document.createElement("tr");
          const status = app.priority;
          tr.innerHTML = `
            <td title="${app.applicant}">${app.companyName || app.applicant}</td>
            <td>${app.type}</td>
            <td>${app.submitted}</td>
            <td>${priorityBadge(app.priority)}</td>
            <td style="white-space:nowrap;">
              <button class="btn small primary" type="button" data-quick="approve" data-app-id="${app.appId}">Quick Approve</button>
              <button class="btn small" type="button" data-quick="review" data-app-id="${app.appId}">Review</button>
            </td>
          `;
          tbody.appendChild(tr);
          tr.querySelectorAll("button").forEach((b) => {
            b.addEventListener("click", () => {
              const act = b.dataset.quick;
              showToast(act === "approve" ? "Approved (mock)" : "Review opened (mock)", `${app.applicant} • ${app.type}`, act === "approve" ? "gold" : "blue");
            });
          });
        });

        // If nothing, show placeholder
        if (!queue.length) {
          tbody.innerHTML = `<tr><td colspan="5" style="color:var(--muted2);padding:18px;">No pending approvals in mock data.</td></tr>`;
        }
      }
      buildDashboardApprovalQueue();

      // ---------------------------
      // Rendering: Registry table
      // ---------------------------
      const registrySearch = $("#registrySearch");
      const registryZone = $("#registryZone");
      const registryEntityType = $("#registryEntityType");
      const registryStatus = $("#registryStatus");
      const registryIndustry = $("#registryIndustry");
      const registryYear = $("#registryYear");

      function initRegistryFilters() {
        const zones = Array.from(new Set(mock.businesses.map((b) => b.zone))).sort();
        const types = Array.from(new Set(mock.businesses.map((b) => b.entityType))).sort();
        const statuses = Array.from(new Set(mock.businesses.map((b) => b.status))).sort();
        const industries = Array.from(new Set(mock.businesses.map((b) => b.industry))).sort();
        const years = Array.from(
          new Set(mock.businesses.map((b) => extractYearFromLicenseExp(b.licenseExp)).filter(Boolean))
        ).sort((a, b) => Number(a) - Number(b));

        zones.forEach((z) => registryZone.insertAdjacentHTML("beforeend", `<option value="${z}">${z}</option>`));
        types.forEach((t) => registryEntityType.insertAdjacentHTML("beforeend", `<option value="${t}">${t}</option>`));
        statuses.forEach((s) => registryStatus.insertAdjacentHTML("beforeend", `<option value="${s}">${s}</option>`));
        industries.forEach((i) => registryIndustry.insertAdjacentHTML("beforeend", `<option value="${i}">${i}</option>`));
        years.forEach((y) => registryYear.insertAdjacentHTML("beforeend", `<option value="${y}">${y}</option>`));
      }
      initRegistryFilters();

      function renderRegistryTable() {
        const tbody = $("#registryTbody");
        const search = (registrySearch.value || "").trim().toLowerCase();
        const zone = registryZone.value;
        const entityType = registryEntityType.value;
        const status = registryStatus.value;
        const industry = registryIndustry.value;
        const year = registryYear.value;

        let rows = [...mock.businesses];

        if (search) {
          rows = rows.filter((b) => {
            const hay = `${b.companyName} ${b.regNo} ${b.tradeName}`.toLowerCase();
            return hay.includes(search);
          });
        }
        if (zone !== "all") rows = rows.filter((b) => b.zone === zone);
        if (entityType !== "all") rows = rows.filter((b) => b.entityType === entityType);
        if (status !== "all") rows = rows.filter((b) => b.status === status);
        if (industry !== "all") rows = rows.filter((b) => b.industry === industry);
        if (year !== "all") rows = rows.filter((b) => extractYearFromLicenseExp(b.licenseExp) === String(year));

        // apply sorting state
        const sortState = renderRegistryTable._sortState || { key: "regNo", dir: "asc" };
        rows.sort((a, b) => {
          const va = String(a[sortState.key] ?? "");
          const vb = String(b[sortState.key] ?? "");
          const cmp = va.localeCompare(vb, undefined, { numeric: true, sensitivity: "base" });
          return sortState.dir === "asc" ? cmp : -cmp;
        });

        tbody.innerHTML = "";
        rows.forEach((b) => {
          const tr = document.createElement("tr");
          tr.className = "row-actionable";
          tr.dataset.companyId = b.id;
          tr.dataset.companyName = b.companyName;
          tr.innerHTML = `
            <td style="width:46px;">
              <input class="row-check" type="checkbox" data-row-id="${b.id}" aria-label="Select ${b.companyName}" />
            </td>
            <td title="${b.regNo}">${b.regNo}</td>
            <td title="${b.companyName}">${b.companyName}</td>
            <td title="${b.entityType}">${b.entityType}</td>
            <td title="${b.industry}">${b.industry}</td>
            <td title="${b.zone}">${b.zone}</td>
            <td title="${b.licenseExp}">${b.licenseExp}</td>
            <td>${statusPill(b.status)}</td>
            <td title="${b.shareholders}">${b.shareholders}</td>
            <td style="white-space:nowrap;">
              <button class="btn small primary row-view" type="button" data-company-id="${b.id}">View Profile</button>
            </td>
          `;
          tbody.appendChild(tr);

          tr.addEventListener("click", (e) => {
            const target = e.target;
            if (
              target &&
              target.closest &&
              (target.closest("button.row-view") || target.closest("input.row-check"))
            ) {
              return;
            }
            // Clicking row opens drawer
            openBusinessDrawer(b.id, "overview");
          });
          tr.addEventListener("contextmenu", (e) => {
            e.preventDefault();
            openContextMenu(e.clientX, e.clientY, { companyId: b.id, companyName: b.companyName });
          });
          tr.querySelector("button.row-view")?.addEventListener("click", (e) => {
            e.stopPropagation();
            openBusinessDrawer(b.id, "overview");
          });
        });

        // update checkboxes state
        $("#selectAllRegistry").checked = false;
      }

      renderRegistryTable();

      // Sorting registry
      $$(".sortable", $("#registryTbody").closest(".table-wrap")).forEach(() => {});
      $$("section#view-registry .sortable").forEach((th) => {
        th.addEventListener("click", () => {
          const key = th.dataset.sort;
          const state = renderRegistryTable._sortState || { key: "regNo", dir: "asc" };
          if (state.key === key) state.dir = state.dir === "asc" ? "desc" : "asc";
          else {
            state.key = key;
            state.dir = "asc";
          }
          renderRegistryTable._sortState = state;
          renderRegistryTable();
        });
      });

      // Filters
      [registrySearch, registryZone, registryEntityType, registryStatus, registryIndustry, registryYear].forEach((el) => {
        el?.addEventListener("input", renderRegistryTable);
        el?.addEventListener("change", renderRegistryTable);
      });

      // Select all + bulk actions
      const selectAllRegistry = $("#selectAllRegistry");
      selectAllRegistry?.addEventListener("change", () => {
        const checked = selectAllRegistry.checked;
        $$("#registryTbody .row-check").forEach((c) => (c.checked = checked));
      });

      function getSelectedRegistryIds() {
        return $$("#registryTbody .row-check:checked").map((c) => c.dataset.rowId);
      }

      $("#applyBulkBtn")?.addEventListener("click", async () => {
        const action = $("#bulkAction").value;
        const selected = getSelectedRegistryIds();
        if (!selected.length) {
          showToast("No selection", "Select one or more businesses first.", "gold");
          return;
        }
        if (action === "suspend") {
          const ok = window.confirm(`Suspend ${selected.length} selected entities?`);
          if (!ok) return;
          try {
            await requirePin("Suspend Selected", "Suspending selected entities requires PIN.");
            selected.forEach((id) => updateBusinessStatus(id, "Suspended"));
            showToast("Bulk suspended", `${selected.length} entities updated (mock).`, "danger");
          } catch {
            showToast("Cancelled", "Bulk suspend cancelled.", "gold");
          }
          return;
        }
        if (action === "renew") {
          showToast("Renewals queued", `Queued renewals for ${selected.length} entities (mock).`, "blue");
          selected.forEach((id) => updateBusinessStatus(id, "Active"));
          return;
        }
        if (action === "export") {
          showToast("Downloading...", `Exporting ${selected.length} profiles (mock CSV)`, "blue");
          setTimeout(() => showToast("Export ready", "Simulated bulk export completed."), 1000);
          return;
        }
        showToast("Bulk action (mock)", "Action executed (or no-op).", "gold");
      });

      $("#exportCsvBtn")?.addEventListener("click", () => {
        const selected = getSelectedRegistryIds();
        if (!selected.length) {
          showToast("Downloading...", "Exporting all businesses (mock CSV)", "blue");
        } else {
          showToast("Downloading...", `Exporting ${selected.length} selected (mock CSV)`, "blue");
        }
        setTimeout(() => showToast("Export ready", "Simulated CSV download completed."), 950);
      });

      $("#registerNewBtn")?.addEventListener("click", () => {
        showToast("+ Register New Business", "Opened registration wizard (mock).", "gold");
      });

      // ---------------------------
      // Rendering: Applications + Kanban + table
      // ---------------------------
      function renderApplications() {
        const kanCounts = {
          "SUBMITTED": $("#kanCount-submitted"),
          "UNDER REVIEW": $("#kanCount-review"),
          "DOCS PENDING": $("#kanCount-docspending"),
          "APPROVED": $("#kanCount-approved"),
          "REJECTED": $("#kanCount-rejected"),
        };
        const kanBoards = {
          "SUBMITTED": $("#kanSubmitted"),
          "UNDER REVIEW": $("#kanReview"),
          "DOCS PENDING": $("#kanDocsPending"),
          "APPROVED": $("#kanApproved"),
          "REJECTED": $("#kanRejected"),
        };

        Object.values(kanBoards).forEach((el) => (el.innerHTML = ""));
        const byStatus = {};
        mock.applications.forEach((a) => {
          byStatus[a.status] = byStatus[a.status] || [];
          byStatus[a.status].push(a);
        });

        Object.entries(kanBoards).forEach(([status, el]) => {
          const list = byStatus[status] || [];
          kanCounts[status] && (kanCounts[status].textContent = String(list.length));
          el.innerHTML = "";
          list.slice(0, 4).forEach((app) => {
            const card = document.createElement("div");
            card.className = "kan-card";
            const pr = priorityBadge(app.priority);
            card.innerHTML = `
              <div class="top">
                <div style="min-width:0;">
                  <div class="cn" title="${app.applicant}">${app.applicant}</div>
                  <div class="meta">${app.type} • Submitted ${app.submitted}</div>
                </div>
                <div class="reviewer" title="Assigned reviewer">
                  <div class="av">${app.reviewer}</div>
                </div>
              </div>
              <div style="margin-top:10px;">${pr}</div>
              <div class="act">
                <button class="btn small primary" type="button" data-app-view="${app.appId}">View Details</button>
              </div>
            `;
            card.querySelector("button")?.addEventListener("click", () => {
              showToast("Details (mock)", `${app.applicant} • ${app.type}`, "blue");
              openBusinessDrawer(app.companyId, "overview");
            });
            el.appendChild(card);
          });
        });

        // Table
        const tbody = $("#appsTbody");
        tbody.innerHTML = "";
        mock.applications.forEach((app) => {
          const tr = document.createElement("tr");
          tr.dataset.companyId = app.companyId;
          tr.dataset.companyName = app.applicant;
          tr.addEventListener("contextmenu", (e) => {
            e.preventDefault();
            openContextMenu(e.clientX, e.clientY, { companyId: app.companyId, companyName: app.applicant });
          });
          const pctRemaining = app.slaDaysTotal
            ? clamp((app.slaDaysLeft / app.slaDaysTotal) * 100, 0, 100)
            : 50;
          const slaColor = pctRemaining > 50 ? "" : pctRemaining >= 25 ? "amber" : "red";
          const progressClass = pctRemaining > 50 ? "" : slaColor;
          tr.innerHTML = `
            <td>${app.appId}</td>
            <td title="${app.applicant}">${app.applicant}</td>
            <td>${app.type}</td>
            <td>${app.submitted}</td>
            <td>${app.reviewer}</td>
            <td>
              <div class="progress-sla ${pctRemaining > 50 ? "" : pctRemaining >= 25 ? "amber" : "red"}" title="SLA time remaining">
                <div class="fill" style="width:${pctRemaining}%;"></div>
              </div>
              <div style="margin-top:8px;color:var(--muted2);font-size:12px;">${pctRemaining.toFixed(0)}% time</div>
            </td>
            <td>${statusPill(app.status === "SUBMITTED" ? "Pending" : app.status === "UNDER REVIEW" ? "Processing" : app.status === "DOCS PENDING" ? "Processing" : app.status === "APPROVED" ? "Active" : "Suspended")}</td>
            <td style="white-space:nowrap;">
              <button class="btn small primary" type="button" data-app-row="${app.appId}" data-requires-pin="false">View</button>
            </td>
          `;
          tr.querySelector("button")?.addEventListener("click", () => {
            showToast("Viewing application (mock)", `${app.applicant}`, "blue");
            openBusinessDrawer(app.companyId, "overview");
          });
          tbody.appendChild(tr);
        });
      }
      renderApplications();

      // ---------------------------
      // Rendering: Licenses
      // ---------------------------
      let licenseTypeFilter = "all";

      function matchesLicenseGroup(license, group) {
        if (group === "all") return true;
        const t = String(license.type || "").toLowerCase();
        const a = String(license.activities || "").toLowerCase();
        const hay = `${t} ${a}`;

        const groups = {
          Trading: ["trading"],
          Services: ["technology", "consult", "it services", "services"],
          Industrial: ["industrial", "warehouse", "manufacturing"],
          Financial: ["investment", "offshore", "finance"],
          Media: ["media"],
          Healthcare: ["healthcare", "pharma"],
          Technology: ["technology", "it"],
          Logistics: ["logistics", "freight", "warehouse", "cross-dock", "branch"],
          Consulting: ["advisory", "consult"],
          "Food & Beverage": ["food", "beverage", "f&b"],
        };

        const keywords = groups[group] || [];
        return keywords.some((kw) => hay.includes(kw));
      }

      function renderLicenses() {
        const tbody = $("#licensesTbody");
        tbody.innerHTML = "";
        const items = mock.licenses.filter((l) => matchesLicenseGroup(l, licenseTypeFilter));
        items.forEach((l) => {
          const tr = document.createElement("tr");
          tr.className = "row-actionable";
          tr.dataset.companyId = l.companyId;
          tr.dataset.companyName = l.company;
          tr.innerHTML = `
            <td style="width:46px;">
              <input class="row-check" type="checkbox" data-row-id="${l.id}" aria-label="Select license ${l.id}" />
            </td>
            <td title="${l.id}">${l.id}</td>
            <td title="${l.company}">${l.company}</td>
            <td>${l.type}</td>
            <td title="${l.activities}">${l.activities}</td>
            <td>${l.issue}</td>
            <td>${l.expiry}</td>
            <td>
              <span class="pill ${l.status === "Active" ? "active" : l.status === "Suspended" ? "suspended" : l.status === "Processing" ? "processing" : "pending"}">
                ${l.renewalStatus}
              </span>
            </td>
            <td style="white-space:nowrap;">
              <button class="btn small primary" type="button" data-license-view="${l.id}">Manage</button>
              <button class="btn small danger" type="button" data-requires-pin="true" data-action-pin="Force Suspend" data-license-suspend="${l.id}">Suspend</button>
            </td>
          `;
          tr.querySelector("button[data-license-view]")?.addEventListener("click", () => {
            const biz = mock.businesses.find((b) => b.id === l.companyId);
            if (biz) openBusinessDrawer(biz.id, "licenses");
            showToast("License management", `Opened ${l.id} (mock).`, "blue");
          });
          tr.querySelector("button[data-license-suspend]")?.addEventListener("click", async () => {
            const ok = window.confirm(`Force suspend license ${l.id}?`);
            if (!ok) return;
            try {
              await requirePin("Force Suspend", "Force suspend requires PIN confirmation.");
              // mock change: if biz exists, set suspended
              updateBusinessStatus(l.companyId, "Suspended");
              showToast("Force suspended", `${l.id} (mock)`, "danger");
            } catch {
              showToast("Cancelled", "PIN confirmation cancelled.", "gold");
            }
          });
          tr.addEventListener("contextmenu", (e) => {
            e.preventDefault();
            openContextMenu(e.clientX, e.clientY, { companyId: l.companyId, companyName: l.company });
          });
          tbody.appendChild(tr);
        });
        if (!items.length) {
          tbody.innerHTML = `<tr><td colspan="9" style="color:var(--muted2);padding:18px;">No licenses match this filter (mock).</td></tr>`;
        }
        $("#selectAllLicenses").checked = false;
      }
      renderLicenses();
      $("#selectAllLicenses")?.addEventListener("change", () => {
        const checked = $("#selectAllLicenses").checked;
        $$("#licensesTbody .row-check").forEach((c) => (c.checked = checked));
      });

      // License filter cards (simple)
      const licenseFilterCards = $$(".filter-cards .fcard");
      licenseFilterCards.forEach((c) => {
        c.addEventListener("click", () => {
          licenseFilterCards.forEach((x) => x.classList.toggle("active", x === c));
          licenseTypeFilter = c.dataset.licenseType || "all";
          showToast("Filter applied", `License type: ${licenseTypeFilter}`, "blue");
          renderLicenses();
        });
      });

      // ---------------------------
      // Rendering: Investors
      // ---------------------------
      function renderInvestors() {
        const grid = $("#investorGrid");
        grid.innerHTML = "";
        mock.investors.forEach((inv) => {
          const card = document.createElement("div");
          card.className = "profile-card card";
          const kycCls = inv.kycStatus === "Verified" ? "active" : inv.kycStatus === "Pending" ? "pending" : "suspended";
          card.innerHTML = `
            <div class="profile-top">
              <div class="av" aria-hidden="true">${inv.name.split(" ").map((x) => x[0]).slice(0, 2).join("")}</div>
              <div class="nm">
                <div class="a" title="${inv.name}">${inv.name}</div>
                <div class="b">${inv.nationality}</div>
              </div>
            </div>
            <div class="profile-kv">
              <div class="row">
                <div class="k">Passport</div>
                <div class="v" title="Masked passport #">${inv.passportMasked}</div>
              </div>
              <div class="row">
                <div class="k">KYC Status</div>
                <div class="v">${inv.kycStatus} <span class="pill ${kycCls}" style="padding:4px 9px;font-size:12px;margin-left:8px;">${inv.kycStatus === "Verified" ? "Verified ✅" : inv.kycStatus === "Pending" ? "Pending 🟡" : "Expired 🔴"}</span></div>
              </div>
              <div class="row">
                <div class="k">Associated companies</div>
                <div class="v">${inv.associated.length} linked</div>
              </div>
            </div>
            <div class="profile-act">
              <button class="btn small primary" type="button" data-inv-view="${inv.id}">View Profile</button>
            </div>
          `;
          card.querySelector("button[data-inv-view]")?.addEventListener("click", () => openInvestorModal(inv.id));
          grid.appendChild(card);
        });
      }
      renderInvestors();

      // ---------------------------
      // Rendering: Infrastructure map
      // ---------------------------
      const infraSVG = $("#infraZoneSectors");
      const infra = [
        { key: "Zone A", path: "M80 40 H320 V120 H80 Z", occupancy: 0.87, units: ["Office 2-09", "Warehouse W3-12"], breakdown: { office: 48, warehouse: 34, land: 18 }, tenants: 62, utilities: { power: "green", water: "amber", internet: "green" } },
        { key: "Zone B", path: "M330 40 H560 V120 H330 Z", occupancy: 0.62, units: ["Office 1-02", "Office 1-05"], breakdown: { office: 44, warehouse: 22, land: 34 }, tenants: 39, utilities: { power: "green", water: "green", internet: "amber" } },
        { key: "Zone C", path: "M80 130 H270 V210 H80 Z", occupancy: 0.45, units: ["Clinic Wing 4", "Lab Unit 8"], breakdown: { office: 28, warehouse: 26, land: 46 }, tenants: 21, utilities: { power: "amber", water: "amber", internet: "green" } },
        { key: "Tech Cluster", path: "M290 130 H560 V210 H290 Z", occupancy: 0.91, units: ["Innovation Pod 7", "Studio 12"], breakdown: { office: 60, warehouse: 12, land: 28 }, tenants: 74, utilities: { power: "amber", water: "green", internet: "green" } },
        { key: "Logistics Hub", path: "M570 40 H780 V120 H570 Z", occupancy: 0.78, units: ["Warehouse Row B-1", "Cross-dock C-3"], breakdown: { office: 18, warehouse: 62, land: 20 }, tenants: 58, utilities: { power: "green", water: "amber", internet: "amber" } },
        { key: "Industrial Area", path: "M620 130 H820 V210 H620 Z", occupancy: 0.53, units: ["Manufacturing Bay 3", "Plant 9"], breakdown: { office: 22, warehouse: 34, land: 44 }, tenants: 33, utilities: { power: "amber", water: "green", internet: "red" } },
      ];

      function renderInfraSVG() {
        infraSVG.innerHTML = "";
        infra.forEach((z) => {
          const color = occupancyFill(z.occupancy);
          const sector = document.createElementNS("http://www.w3.org/2000/svg", "path");
          sector.setAttribute("d", z.path);
          sector.setAttribute("fill", color);
          sector.setAttribute("stroke", "rgba(201,168,76,0.55)");
          sector.setAttribute("stroke-width", "2");
          sector.setAttribute("class", "zone-sector");
          sector.dataset.zone = z.key;
          sector.dataset.occupancy = String(z.occupancy);
          sector.addEventListener("click", () => showInfraZone(z.key));
          infraSVG.appendChild(sector);

          const text = document.createElementNS("http://www.w3.org/2000/svg", "text");
          text.setAttribute("x", z.key === "Zone A" ? "200" : z.key === "Zone B" ? "445" : z.key === "Zone C" ? "175" : z.key === "Tech Cluster" ? "425" : z.key === "Logistics Hub" ? "675" : "720");
          text.setAttribute("y", z.key === "Zone A" || z.key === "Zone B" || z.key === "Logistics Hub" ? "85" : "175");
          text.setAttribute("font-family", "JetBrains Mono, monospace");
          text.setAttribute("font-size", "13");
          text.setAttribute("fill", "rgba(234,240,255,0.92)");
          text.setAttribute("text-anchor", "middle");
          text.textContent = z.key;
          infraSVG.appendChild(text);
        });
      }

      function utilStatusColor(s) {
        if (s === "green") return "rgba(47,227,139,0.7)";
        if (s === "amber") return "rgba(242,184,75,0.85)";
        return "rgba(227,93,106,0.85)";
      }

      function renderUtilities(utilities) {
        const host = $("#infraUtilities");
        host.innerHTML = "";
        Object.entries(utilities).forEach(([k, v]) => {
          const el = document.createElement("div");
          el.className = "util-pill" + (v === "amber" ? " amber" : v === "red" ? " red" : "");
          el.innerHTML = `
            <span class="s" style="background:${utilStatusColor(v)};"></span>
            <span>${k[0].toUpperCase() + k.slice(1)}: ${v.toUpperCase()}</span>
          `;
          host.appendChild(el);
        });
      }

      function showInfraZone(key) {
        const z = infra.find((x) => x.key === key);
        if (!z) return;
        $("#infraZoneName").textContent = key;
        $("#infraZonePct").textContent = `${Math.round(z.occupancy * 100)}% occupied`;
        $("#infraZoneBarFill").style.width = `${Math.round(z.occupancy * 100)}%`;

        // breakdown text
        const bd = z.breakdown;
        $("#infraUnitBreakdown").textContent = `${bd.office} office • ${bd.warehouse} warehouse • ${bd.land} land (mock)`;
        $("#infraTenantCount").textContent = `${z.tenants} tenants`;
        renderUtilities(z.utilities);
        renderUnitsTable(key);

        // Derive tenant list from rendered unit rows (mock)
        const tenantSet = new Set();
        $$("#unitsTbody tr").forEach((tr) => {
          const td = tr.children[3];
          const t = td ? String(td.textContent || "").trim() : "";
          if (t) tenantSet.add(t);
        });
        const availableUnits = Math.max(0, 120 - Number(z.tenants || 0));
        $("#infraTenantDetails").textContent = `Tenants: ${Array.from(tenantSet).slice(0, 3).join(", ") || "—"} • Available units: ${availableUnits}`;
      }

      function renderUnitsTable(zoneKey) {
        const units = [
          { id: "U-A-102", type: "Office Suite", area: 980, tenant: "Al Masdar Trading LLC", leaseStart: "2023-06-01", leaseEnd: "2026-05-31", rent: "$146,000", status: "Active" },
          { id: "U-A-331", type: "Warehouse Unit", area: 2450, tenant: "Meridian Foods", leaseStart: "2021-01-15", leaseEnd: "2026-01-14", rent: "$320,000", status: "Active" },
          { id: "U-B-077", type: "Office Suite", area: 760, tenant: "Vertex Holdings", leaseStart: "2024-04-20", leaseEnd: "2027-04-19", rent: "$118,000", status: "Active" },
          { id: "U-C-014", type: "Lab Unit", area: 1100, tenant: "Delta Pharma", leaseStart: "2022-11-30", leaseEnd: "2025-11-29", rent: "$185,000", status: "Active" },
          { id: "U-D-208", type: "Innovation Pod", area: 640, tenant: "NovaTech Solutions", leaseStart: "2024-10-07", leaseEnd: "2027-10-06", rent: "$92,000", status: "Active" },
          { id: "U-E-509", type: "Cross-dock Bay", area: 1500, tenant: "BlueSky Logistics", leaseStart: "2023-02-14", leaseEnd: "2026-02-13", rent: "$240,000", status: "Active" },
          { id: "U-F-903", type: "Manufacturing Bay", area: 3200, tenant: "Atlas Energy Partners", leaseStart: "2022-06-01", leaseEnd: "2025-05-31", rent: "$510,000", status: "Active" },
        ];
        // simple mapping
        const mapping = {
          "Zone A": ["U-A-102", "U-A-331"],
          "Zone B": ["U-B-077"],
          "Zone C": ["U-C-014"],
          "Tech Cluster": ["U-D-208"],
          "Logistics Hub": ["U-E-509"],
          "Industrial Area": ["U-F-903"],
        };
        const allowed = new Set(mapping[zoneKey] || []);
        const rows = units.filter((u) => allowed.has(u.id));
        const tbody = $("#unitsTbody");
        tbody.innerHTML = "";
        rows.forEach((u) => {
          const tr = document.createElement("tr");
          tr.innerHTML = `
            <td>${u.id}</td>
            <td>${u.type}</td>
            <td>${u.area.toLocaleString()}</td>
            <td title="${u.tenant}">${u.tenant}</td>
            <td>${u.leaseStart}</td>
            <td>${u.leaseEnd}</td>
            <td>${u.rent}</td>
            <td>${statusPill(u.status === "Active" ? "Active" : "Pending")}</td>
          `;
          tr.addEventListener("contextmenu", (e) => {
            e.preventDefault();
            openContextMenu(e.clientX, e.clientY, { companyName: u.tenant, companyId: findBusinessIdByCompanyName(u.tenant) });
          });
          tbody.appendChild(tr);
        });
        if (!rows.length) {
          tbody.innerHTML = `<tr><td colspan="8" style="color:var(--muted2);padding:18px;">No unit records in mock data for ${zoneKey}.</td></tr>`;
        }
      }

      renderInfraSVG();
      showInfraZone("Zone A");

      // Global hero zone activity map click in dashboard
      $$("#zoneSectors .zone-sector").forEach((p) => {
        p.addEventListener("click", () => {
          const z = mock.zones.find((x) => {
            const label = x.name === "Zone A" ? "Zone A" : x.name;
            return label === p.dataset.zone;
          });
          const occ = parseFloat(p.dataset.occupancy || "0.5");
          const color = getOccupancyColor(occ);
          $("#zoneBarFill").style.width = `${Math.round(occ * 100)}%`;
          const bd = z ? z.breakdown : { office: 0, warehouse: 0, land: 0 };
          $("#zoneBreakdown").textContent = `office ${bd.office}% • warehouse ${bd.warehouse}% • land ${bd.land}% (mock)`;
          $("#zoneTenantCount").textContent = z ? `${z.tenants} tenants` : "—";
          const items = $("#zoneUtilities");
          items.innerHTML = "";
          if (z) {
            const utils = z.utilities;
            Object.entries(utils).forEach(([k, v]) => {
              const el = document.createElement("div");
              el.className = "util-pill" + (v === "amber" ? " amber" : v === "red" ? " red" : "");
              el.innerHTML = `
                <span class="s"></span>
                <span>${k[0].toUpperCase() + k.slice(1)}: ${v.toUpperCase()}</span>
              `;
              items.appendChild(el);
            });
          }

          // highlight selected
          $$("#zoneSectors .zone-sector").forEach((pp) => pp.style.opacity = "0.65");
          p.style.opacity = "1";
          showToast("Zone selected", `${p.dataset.zone} occupancy ${(occ * 100).toFixed(0)}%`, color === "red" ? "danger" : color === "amber" ? "gold" : "blue");
        });
      });

      // ---------------------------
      // Rendering: Dashboard map initial coloring
      // ---------------------------
      function initDashboardMap() {
        const sectors = $$("#zoneSectors .zone-sector");
        sectors.forEach((s) => {
          const occ = parseFloat(s.dataset.occupancy || "0.5");
          s.setAttribute("fill", occupancyFill(occ));
        });
        // default detail for Zone A
        const z = mock.zones[0];
        if (!z) return;
        const occ = z.occupancy;
        $("#zoneBarFill").style.width = `${Math.round(occ * 100)}%`;
        $("#zoneTenantCount").textContent = `${z.tenants} tenants`;
        const bd = z.breakdown;
        $("#zoneBreakdown").textContent = `office ${bd.office}% • warehouse ${bd.warehouse}% • land ${bd.land}% (mock)`;
        $("#zoneUtilities").innerHTML = "";
        Object.entries(z.utilities).forEach(([k, v]) => {
          const el = document.createElement("div");
          el.className = "util-pill" + (v === "amber" ? " amber" : v === "red" ? " red" : "");
          el.innerHTML = `<span class="s"></span><span>${k[0].toUpperCase() + k.slice(1)}: ${v.toUpperCase()}</span>`;
          $("#zoneUtilities").appendChild(el);
        });
      }
      initDashboardMap();

      // ---------------------------
      // Revenue bars / trend charts (CSS-only visualizations sized in JS)
      // ---------------------------
      function renderRevenueBars() {
        const host = $("#revenueBars");
        host.innerHTML = "";
        const months = ["Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec", "Jan", "Feb", "Mar"];
        const values = [220, 245, 260, 240, 270, 290, 310, 295, 330, 350, 325, 360]; // mock
        const max = Math.max(...values);
        months.forEach((m, i) => {
          const pct = (values[i] / max) * 100;
          const bar = document.createElement("div");
          bar.className = "bar";
          bar.style.height = `${pct}%`;
          bar.title = `${m}: $${values[i]}k`;
          host.appendChild(bar);
        });
      }
      renderRevenueBars();

      function renderTrendChart() {
        const host = $("#trendChart");
        host.innerHTML = "";
        const values = [18, 22, 19, 25, 28, 30, 27, 32, 35, 33, 38, 40]; // mock
        const max = Math.max(...values);
        values.forEach((v, i) => {
          const pct = (v / max) * 100;
          const bar = document.createElement("div");
          bar.className = "bar";
          bar.style.height = `${pct}%`;
          bar.style.background = "linear-gradient(180deg, rgba(201,168,76,0.8), rgba(77,163,255,0.18))";
          bar.title = `Month ${i + 1}: ${v}%`;
          host.appendChild(bar);
        });
      }
      // trend bars uses .bars CSS, but trendChart container uses .bars styles; adapt:
      renderTrendChart();
      // Ensure container has .bars class styling
      $("#trendChart").classList.add("bars");

      function renderIndustryBars() {
        const host = $("#industryBars");
        host.innerHTML = "";
        const data = [
          { k: "Commodities", v: 36 },
          { k: "Technology", v: 28 },
          { k: "Logistics", v: 21 },
          { k: "Healthcare", v: 15 },
          { k: "F&B", v: 12 },
        ];
        const max = Math.max(...data.map((d) => d.v));
        data.forEach((d) => {
          const pct = (d.v / max) * 100;
          const row = document.createElement("div");
          row.style.display = "grid";
          row.style.gridTemplateColumns = "130px 1fr 56px";
          row.style.gap = "10px";
          row.style.alignItems = "center";
          row.style.marginBottom = "10px";
          row.innerHTML = `
            <div style="color:var(--muted2);font-family:var(--serif);font-size:14px;">${d.k}</div>
            <div style="height:12px;border-radius:999px;background:rgba(120,150,255,0.12);border:1px solid rgba(120,150,255,0.12);overflow:hidden;">
              <div style="height:100%;width:${pct}%;background:linear-gradient(90deg, rgba(201,168,76,0.55), rgba(77,163,255,0.25));"></div>
            </div>
            <div style="color:var(--text);font-weight:800;font-size:12px;text-align:right;">${d.v}%</div>
          `;
          host.appendChild(row);
        });
      }
      renderIndustryBars();

      function renderNationalityHeatmap() {
        const host = $("#nationalityHeatmap");
        host.innerHTML = "";
        const data = [
          { nat: "🇦🇪", k: "UAE", v: 26 },
          { nat: "🇮🇳", k: "India", v: 17 },
          { nat: "🇸🇬", k: "Singapore", v: 12 },
          { nat: "🇩🇪", k: "Germany", v: 9 },
          { nat: "🇯🇵", k: "Japan", v: 8 },
          { nat: "🇸🇦", k: "Saudi", v: 7 },
          { nat: "🇬🇧", k: "UK", v: 6 },
          { nat: "🇨🇦", k: "Canada", v: 5 },
          { nat: "🇪🇬", k: "Egypt", v: 4 },
          { nat: "🇳🇬", k: "Nigeria", v: 3 },
        ];
        const max = Math.max(...data.map((d) => d.v));
        data.forEach((d) => {
          const pct = d.v / max;
          const bg =
            pct > 0.7 ? "rgba(201,168,76,0.18)" : pct > 0.4 ? "rgba(77,163,255,0.16)" : "rgba(120,150,255,0.10)";
          const border = pct > 0.7 ? "rgba(201,168,76,0.32)" : pct > 0.4 ? "rgba(77,163,255,0.28)" : "rgba(120,150,255,0.18)";
          const cell = document.createElement("div");
          cell.style.display = "inline-flex";
          cell.style.alignItems = "center";
          cell.style.gap = "8px";
          cell.style.padding = "10px 12px";
          cell.style.borderRadius = "16px";
          cell.style.border = `1px solid ${border}`;
          cell.style.background = bg;
          cell.style.margin = "0 8px 10px 0";
          cell.style.color = "var(--text)";
          cell.style.fontSize = "12px";
          cell.title = `${d.k}: ${d.v}%`;
          cell.innerHTML = `<span style="font-size:16px;">${d.nat}</span><span style="font-weight:900;">${d.k}</span><span style="color:var(--muted2)">${d.v}</span>`;
          host.appendChild(cell);
        });
      }
      renderNationalityHeatmap();

      function renderRevenueVsTarget() {
        const host = $("#revenueVsTarget");
        host.innerHTML = "";
        const data = [
          { k: "Apr", a: 220, b: 210 },
          { k: "May", a: 245, b: 235 },
          { k: "Jun", a: 260, b: 250 },
          { k: "Jul", a: 240, b: 260 },
          { k: "Aug", a: 270, b: 265 },
        ];
        const max = Math.max(...data.map((d) => Math.max(d.a, d.b)));
        data.forEach((d, i) => {
          const pctA = (d.a / max) * 100;
          const pctB = (d.b / max) * 100;
          const wrap = document.createElement("div");
          wrap.style.flex = "1";
          wrap.style.minWidth = "64px";
          wrap.style.display = "grid";
          wrap.style.alignItems = "end";
          wrap.style.gridTemplateRows = "1fr auto";
          wrap.style.gap = "10px";
          wrap.innerHTML = `
            <div style="display:flex;align-items:flex-end;gap:8px;height:140px;">
              <div title="Actual" style="flex:1;height:${pctA}%;background:linear-gradient(180deg, rgba(201,168,76,0.75), rgba(77,163,255,0.22));border:1px solid rgba(201,168,76,0.18);border-radius:12px;"></div>
              <div title="Target" style="flex:1;height:${pctB}%;background:rgba(120,150,255,0.12);border:1px solid rgba(120,150,255,0.14);border-radius:12px;"></div>
            </div>
            <div style="color:var(--muted2);font-size:12px;text-align:center;">${d.k}</div>
          `;
          host.appendChild(wrap);
        });
        host.style.display = "flex";
        host.style.gap = "10px";
      }
      renderRevenueVsTarget();

      // ---------------------------
      // Fee schedule / invoices tables
      // ---------------------------
      function renderFeeTables() {
        const feeBody = $("#feeScheduleTbody");
        feeBody.innerHTML = "";
        mock.feeSchedule.forEach((f) => {
          const tr = document.createElement("tr");
          tr.innerHTML = `
            <td>${f.service}</td>
            <td>${f.entityType}</td>
            <td>$${f.fee.toLocaleString()}</td>
            <td>${Math.round(f.vat * 100)}%</td>
            <td>$${(f.total).toLocaleString(undefined, { maximumFractionDigits: 2 })}</td>
            <td><button class="btn small" type="button" data-requires-pin="true" data-pin-action="Preview Fee">Preview</button></td>
          `;
          feeBody.appendChild(tr);
        });

        const invBody = $("#invoicesTbody");
        invBody.innerHTML = "";
        mock.invoices.forEach((inv) => {
          const tr = document.createElement("tr");
          let pillCls = "pending";
          if (inv.status.includes("Paid")) pillCls = "active";
          else if (inv.status.includes("Overdue")) pillCls = "suspended";
          else if (inv.status.includes("Disputed")) pillCls = "processing";
          else if (inv.status.includes("Waived")) pillCls = "active";
          tr.innerHTML = `
            <td>${inv.id}</td>
            <td title="${inv.company}">${inv.company}</td>
            <td>${inv.service}</td>
            <td>${inv.amount}</td>
            <td>${inv.due}</td>
            <td>${inv.status ? `<span class="pill ${pillCls}">${inv.status}</span>` : "—"}</td>
            <td style="white-space:nowrap;">
              <button class="btn small primary" type="button" data-requires-pin="false" data-action-pin="View Invoice">View</button>
              <button class="btn small danger" type="button" data-requires-pin="true" data-pin-action="Apply Waiver">Waive</button>
            </td>
          `;
          tr.querySelectorAll("button").forEach((b) => {
            b.addEventListener("click", async () => {
              const req = b.dataset.requiresPin === "true";
              const pinAction = b.dataset.pinAction || b.dataset.actionPin || "Action";
              if (req) {
                const ok = window.confirm(`Proceed: ${pinAction}?`);
                if (!ok) return;
                try {
                  await requirePin(pinAction, `PIN required for: ${pinAction}`);
                } catch {
                  showToast("Cancelled", "PIN cancelled.", "gold");
                  return;
                }
              }
              if (b.textContent.includes("Waive")) showToast("Waiver applied (mock)", `${inv.id}`, "gold");
              else showToast("Invoice opened (mock)", `${inv.id}`, "blue");
            });
          });
          tr.addEventListener("contextmenu", (e) => {
            e.preventDefault();
            openContextMenu(e.clientX, e.clientY, { companyName: inv.company, companyId: findBusinessIdByCompanyName(inv.company) });
          });
          invBody.appendChild(tr);
        });
      }
      renderFeeTables();

      // ---------------------------
      // Global override buttons that require PIN
      // ---------------------------
      function bindPinActionButtons(root = document) {
        $$("[data-requires-pin='true'][data-pin-action]", root).forEach((btn) => {
          btn.addEventListener("click", async () => {
            const actionName = btn.dataset.pinAction;
            const ok = window.confirm(`⚠️ ${actionName}. Proceed (mock)?`);
            if (!ok) return;
            try {
              await requirePin(actionName, `PIN required for: ${actionName}`);
              showToast("Authority override", `${actionName} executed (mock).`, "gold");
            } catch {
              showToast("Cancelled", "PIN confirmation cancelled.", "gold");
            }
          });
        });
      }
      bindPinActionButtons(document);

      // ---------------------------
      // Applications: dashboard action handlers
      // ---------------------------
      $$("[data-quick]").forEach((btn) => {
        btn.addEventListener("click", () => {
          showToast("Action queued (mock)", "Approval queue updated.", "gold");
        });
      });

      // ---------------------------
      // Bulk select checkboxes (licenses and registry)
      // ---------------------------
      // (UI already handles selection; not adding more)

      // ---------------------------
      // Export toast simulation for any export-like buttons
      // ---------------------------
      $$("button, .btn").forEach((b) => {
        const t = (b.textContent || "").toLowerCase();
        if (t.includes("export") || t.includes("download")) {
          b.addEventListener("click", () => {
            // Only show if not already a modal action
            if (!b.dataset.pinAction) showToast("Downloading...", "Simulated export in progress...", "blue");
          });
        }
      });

      // ---------------------------
      // Smooth scroll fade on load (stagger)
      // ---------------------------
      function initStaggerOnLoad() {
        const items = $$(".fade-stagger");
        items.forEach((el) => {
          el.classList.remove("visible");
          el.style.transitionDelay = `${Math.random() * 140}ms`;
        });
        setTimeout(() => {
          const active = $("section.view.active") || views[0];
          const scoped = active ? $$("#" + active.id + " .fade-stagger") : items;
          scoped.forEach((el) => {
            el.classList.add("visible");
          });
        }, 60);
      }
      // Ensure dashboard is visible first
      setActiveView("dashboard");
      initStaggerOnLoad();

      // ---------------------------
      // Global search across mock entities
      // ---------------------------
      const globalSearch = $("#globalSearch");
      const searchResults = $("#searchResults");

      const searchIndex = {
        businesses: mock.businesses.map((b) => ({ id: b.id, type: "Company", label: b.companyName, sub: `${b.regNo} • ${b.entityType}`, targetView: "registry", action: () => openBusinessDrawer(b.id) })),
        investors: mock.investors.map((i) => ({ id: i.id, type: "Investor", label: i.name, sub: `${i.nationality} • KYC: ${i.kycStatus}`, targetView: "investors", action: () => openInvestorModal(i.id) })),
        licenses: mock.licenses.map((l) => ({ id: l.id, type: "License", label: l.id, sub: `${l.company} • ${l.type}`, targetView: "licenses", action: () => { setActiveView("licenses"); showToast("License selected", l.id, "blue"); const biz = mock.businesses.find((b) => b.id === l.companyId); if (biz) openBusinessDrawer(biz.id, "licenses"); } })),
        invoices: mock.invoices.map((inv) => ({ id: inv.id, type: "Invoice", label: inv.id, sub: `${inv.company} • ${inv.service}`, targetView: "fees", action: () => { setActiveView("fees"); showToast("Invoice found", inv.id, "blue"); } })),
      };

      const allSearchItems = [
        ...searchIndex.businesses,
        ...searchIndex.investors,
        ...searchIndex.licenses,
        ...searchIndex.invoices,
      ];

      let searchTimer = null;

      function runSearch(q) {
        const query = q.trim().toLowerCase();
        if (!query) {
          searchResults.classList.remove("show");
          searchResults.innerHTML = "";
          return;
        }
        const matches = allSearchItems
          .filter((it) => `${it.label} ${it.sub} ${it.type}`.toLowerCase().includes(query))
          .slice(0, 8);

        searchResults.innerHTML = "";
        if (!matches.length) {
          searchResults.innerHTML = `<div class="item" style="cursor:default;color:var(--muted2);">No results.</div>`;
        } else {
          matches.forEach((m) => {
            const el = document.createElement("div");
            el.className = "item";
            el.setAttribute("role", "option");
            el.innerHTML = `
              <div class="pill">${m.type}</div>
              <div class="txt">
                <div class="a">${m.label}</div>
                <div class="b">${m.sub}</div>
              </div>
            `;
            el.addEventListener("click", () => {
              searchResults.classList.remove("show");
              globalSearch.value = m.label;
              setActiveView(m.targetView);
              setTimeout(() => m.action(), 30);
            });
            searchResults.appendChild(el);
          });
        }
        searchResults.classList.add("show");
      }

      globalSearch?.addEventListener("input", () => {
        window.clearTimeout(searchTimer);
        searchTimer = window.setTimeout(() => runSearch(globalSearch.value), 120);
      });
      globalSearch?.addEventListener("focus", () => {
        if (globalSearch.value.trim()) searchResults.classList.add("show");
      });
      document.addEventListener("click", (e) => {
        if (e.target !== globalSearch && !searchResults.contains(e.target)) searchResults.classList.remove("show");
      });

      globalSearch?.addEventListener("keydown", (e) => {
        if (e.key === "Escape") {
          searchResults.classList.remove("show");
          globalSearch.blur();
        }
      });

      // ---------------------------
      // Dashboard KPI animated count-up
      // ---------------------------
      function countUp(el, target, durationMs) {
        const start = performance.now();
        const from = 0;
        function tick(now) {
          const t = clamp((now - start) / durationMs, 0, 1);
          const eased = 1 - Math.pow(1 - t, 3);
          const val = from + (target - from) * eased;
          el.textContent = String(val).includes(".") ? val.toFixed(0) : Math.round(val).toLocaleString();
          if (t < 1) requestAnimationFrame(tick);
        }
        requestAnimationFrame(tick);
      }

      function initKpiCountUp() {
        const kpis = $$("[data-count]");
        kpis.forEach((el, idx) => {
          const raw = el.dataset.count;
          const num = Number(raw.replace(/[$,]/g, ""));
          const dur = 900 + idx * 100;
          // Keep label format where possible
          if (raw && raw.toString().includes("$")) {
            // not expected
          }
          const isMoney = (el.textContent || "").includes("$");
          const prefix = (el.textContent || "").includes("$") ? "$" : "";
          const target = num;
          const start = performance.now();
          function tick(now) {
            const t = clamp((now - start) / dur, 0, 1);
            const eased = 1 - Math.pow(1 - t, 3);
            const v = startVal + (target - startVal) * eased;
          }
        });
      }

      // Correct count-up:
      function initKpis() {
        const kpis = $$(".kpi-card [data-count]");
        kpis.forEach((el, idx) => {
          const target = Number(String(el.dataset.count).replace(/[$,]/g, ""));
          const dur = 1100 + idx * 120;
          const isMoney = (el.textContent || "").includes("$") || target >= 1000000;
          const format = (v) => {
            if (isMoney && (el.textContent || "").includes("$")) {
              // show in M if target is in millions
              if (target >= 1000000) return `$${(v / 1000000).toFixed(1)}M`;
              return `$${Math.round(v).toLocaleString()}`;
            }
            return Math.round(v).toLocaleString();
          };
          const start = performance.now();
          const startVal = 0;
          function tick(now) {
            const t = clamp((now - start) / dur, 0, 1);
            const eased = 1 - Math.pow(1 - t, 3);
            const v = startVal + (target - startVal) * eased;
            el.textContent = format(v);
            if (t < 1) requestAnimationFrame(tick);
          }
          requestAnimationFrame(tick);
        });
      }

      initKpis();

      // ---------------------------
      // Handle Export buttons precisely (simulated)
      // ---------------------------
      $$(".btn, button").forEach((btn) => {
        const txt = (btn.textContent || "").toLowerCase();
        if (txt.includes("export") || txt.includes("download") || txt.includes("generate batch certificates") || txt.includes("download pdf")) {
          btn.addEventListener("click", (e) => {
            if (btn.dataset.requiresPin === "true") return; // already handled with pin
            if (txt.includes("export")) {
              showToast("Downloading...", "Simulated export started...", "blue");
            }
          });
        }
      });

      // ---------------------------
      // Compliance rendering (risk matrix + tables)
      // ---------------------------
      function severityToDotClass(severity) {
        const s = String(severity || "").toUpperCase();
        if (s === "HIGH") return "red";
        if (s === "AMBER") return "amber";
        return ""; // LOW -> green by CSS default
      }

      function severityToPriorityClass(severity) {
        const s = String(severity || "").toUpperCase();
        if (s === "HIGH") return "high";
        if (s === "AMBER") return "medium";
        return "low";
      }

      function renderCompliance() {
        // Risk matrix
        const host = $("#riskMatrix");
        host.innerHTML = "";

        const likelihood = ["Low", "Medium", "High"];
        const impact = ["Low", "Medium", "High"];

        const cellDots = new Map();
        for (let li = 0; li < 3; li++) {
          for (let im = 0; im < 3; im++) {
            const key = `${li}-${im}`;
            const cell = document.createElement("div");
            cell.className = "risk-cell";
            cell.dataset.li = String(li);
            cell.dataset.im = String(im);
            cell.innerHTML = `
              <div class="lab">${likelihood[li]} Likelihood • ${impact[im]} Impact</div>
              <div class="dots"></div>
            `;
            host.appendChild(cell);
            cellDots.set(key, cell.querySelector(".dots"));
          }
        }

        const riskDots = [
          { company: "Horizon Capital Offshore", severity: "HIGH", li: 2, im: 2 },
          { company: "Vertex Holdings Ltd", severity: "HIGH", li: 1, im: 2 },
          { company: "Pacific Rim Exports Ltd", severity: "AMBER", li: 0, im: 2 },
          { company: "Atlas Energy Partners", severity: "HIGH", li: 2, im: 1 },
          { company: "BlueSky Logistics Corp", severity: "LOW", li: 0, im: 0 },
          { company: "NovaTech Solutions FZE", severity: "AMBER", li: 1, im: 1 },
          { company: "GreenTech Innovations FZE", severity: "AMBER", li: 0, im: 1 },
          { company: "Al Masdar Trading LLC", severity: "LOW", li: 1, im: 0 },
        ];

        riskDots.forEach((d) => {
          const target = cellDots.get(`${d.li}-${d.im}`);
          if (!target) return;
          const dot = document.createElement("div");
          dot.className = `risk-dot ${severityToDotClass(d.severity)}`.trim();
          dot.title = `${d.company} • ${d.severity}`;
          target.appendChild(dot);
        });

        // Flagged companies table
        const flaggedTbody = $("#flaggedTbody");
        flaggedTbody.innerHTML = "";
        const flags = mock.flaggedCompanies || [];
        if (flags.length) {
          flags.forEach((f) => {
            const cls = severityToPriorityClass(f.severity);
            const tr = document.createElement("tr");
            tr.innerHTML = `
              <td title="${f.company}">${f.company}</td>
              <td title="${f.reason}">${f.reason}</td>
              <td><span class="priority ${cls}">${String(f.severity).toUpperCase()}</span></td>
              <td>${f.since}</td>
              <td>${f.assigned}</td>
              <td>${f.status}</td>
            `;
            tr.addEventListener("contextmenu", (e) => {
              e.preventDefault();
              openContextMenu(e.clientX, e.clientY, {
                companyName: f.company,
                companyId: findBusinessIdByCompanyName(f.company),
              });
            });
            flaggedTbody.appendChild(tr);
          });
        } else {
          flaggedTbody.innerHTML = `<tr><td colspan="6" style="color:var(--muted2);padding:18px;">No flagged companies (mock).</td></tr>`;
        }

        // Audit log table
        const auditTbody = $("#auditTbody");
        auditTbody.innerHTML = "";
        const audit = mock.auditLog || [];
        if (audit.length) {
          audit.forEach((a) => {
            const tr = document.createElement("tr");
            tr.innerHTML = `
              <td>${a.ts}</td>
              <td>${a.action}</td>
              <td>${a.entity}</td>
              <td>${a.by}</td>
              <td>${a.before}</td>
              <td>${a.after}</td>
              <td>${a.ip}</td>
            `;
            tr.addEventListener("contextmenu", (e) => {
              e.preventDefault();
              openContextMenu(e.clientX, e.clientY, {
                companyName: a.entity,
                companyId: findBusinessIdByCompanyName(a.entity),
              });
            });
            auditTbody.appendChild(tr);
          });
        } else {
          auditTbody.innerHTML = `<tr><td colspan="7" style="color:var(--muted2);padding:18px;">No audit records (mock).</td></tr>`;
        }
      }
      renderCompliance();

      // ---------------------------
      // Analytics rendering (report builder + prebuilt report cards)
      // ---------------------------
      function renderAnalytics() {
        // Populate entity + zone filters
        const reportEntityType = $("#reportEntityType");
        const reportZone = $("#reportZone");
        const entityTypes = Array.from(new Set(mock.businesses.map((b) => b.entityType))).sort();
        const zones = Array.from(new Set(mock.businesses.map((b) => b.zone))).sort();

        // Clear except "all"
        reportEntityType.innerHTML = `<option value="all">All</option>`;
        entityTypes.forEach((t) => reportEntityType.insertAdjacentHTML("beforeend", `<option value="${t}">${t}</option>`));
        reportZone.innerHTML = `<option value="all">All</option>`;
        zones.forEach((z) => reportZone.insertAdjacentHTML("beforeend", `<option value="${z}">${z}</option>`));

        const reportCardsHost = $("#reportCards");
        const prebuiltReports = [
          { id: "r1", title: "Monthly Registration Report", last: "2026-03-28" },
          { id: "r2", title: "License Expiry Forecast (Next 90 days)", last: "2026-03-24" },
          { id: "r3", title: "Revenue vs Budget Report", last: "2026-03-20" },
          { id: "r4", title: "Nationality Distribution of Investors", last: "2026-03-12" },
          { id: "r5", title: "Zone Occupancy Report", last: "2026-03-15" },
          { id: "r6", title: "Compliance Risk Summary", last: "2026-03-10" },
          { id: "r7", title: "Visa Utilization by Company", last: "2026-03-05" },
        ];

        reportCardsHost.innerHTML = "";
        prebuiltReports.forEach((r, idx) => {
          const card = document.createElement("div");
          card.className = "card fade-stagger";
          card.style.transitionDelay = `${80 + idx * 35}ms`;
          card.innerHTML = `
            <div class="card-head">
              <div class="h">${r.title}</div>
              <div class="meta">Last generated: ${r.last}</div>
            </div>
            <div class="controls" style="display:flex;gap:10px;flex-wrap:wrap;">
              <button class="btn small primary" type="button" data-report-action="generate" data-report-id="${r.id}">Generate Now</button>
              <button class="btn small" type="button" data-report-action="download" data-report-id="${r.id}">Download PDF</button>
              <button class="btn small blue" type="button" data-report-action="schedule" data-report-id="${r.id}">Schedule Auto-Send</button>
            </div>
          `;
          reportCardsHost.appendChild(card);
        });

        // Ensure staggered elements become visible when view is active
        // (If user navigates away and back, fade triggers will be re-applied.)
        $$(".fade-stagger", reportCardsHost).forEach((el) => {
          setTimeout(() => el.classList.add("visible"), 60);
        });

        // Generate report builder
        $("#generateReportBtn")?.addEventListener("click", () => {
          const start = $("#reportStart").value || "—";
          const end = $("#reportEnd").value || "—";
          const entityType = $("#reportEntityType").value;
          const zone = $("#reportZone").value;
          const type = $("#reportType").value;
          showToast("Report generation queued (mock)", `${type} • ${entityType} • ${zone}`, "gold");
          showToast("Downloading preview…", `Time window: ${start} → ${end}`, "blue");
        });

        reportCardsHost.addEventListener("click", (e) => {
          const btn = e.target.closest("button[data-report-action]");
          if (!btn) return;
          const action = btn.dataset.reportAction;
          const id = btn.dataset.reportId;
          const rep = prebuiltReports.find((x) => x.id === id);
          const title = rep ? rep.title : id;
          if (action === "generate") showToast("Generated report (mock)", title, "gold");
          if (action === "download") showToast("Downloading PDF (mock)", title, "blue");
          if (action === "schedule") showToast("Auto-send scheduled (mock)", title, "blue");
        });
      }
      renderAnalytics();

      // ---------------------------
      // Access & Permissions rendering (users + permission matrix)
      // ---------------------------
      function renderAccessPermissions() {
        // Users
        const usersTbody = $("#usersTbody");
        if (usersTbody) {
          usersTbody.innerHTML = "";
          mock.users.forEach((u) => {
            const tr = document.createElement("tr");
            const twoFa = u.twoFA === "Enabled" ? "Enabled" : "Disabled";
            const statusCls = u.status === "Active" ? "active" : "suspended";
            tr.innerHTML = `
              <td title="${u.user}">${u.user}</td>
              <td>${u.role}</td>
              <td>${u.lastLogin}</td>
              <td>${twoFa}</td>
              <td>${statusPill(u.status)} </td>
              <td style="white-space:nowrap;">
                <button class="btn small ghost" type="button" data-user-action="Manage" title="Mock action">Manage</button>
              </td>
            `;
            tr.addEventListener("contextmenu", (e) => {
              e.preventDefault();
              openContextMenu(e.clientX, e.clientY, { companyName: u.user, companyId: null });
            });
            tr.querySelector("button[data-user-action]")?.addEventListener("click", () => {
              showToast("User action (mock)", `${u.user} • ${u.role}`, "blue");
            });
            usersTbody.appendChild(tr);
          });
        }

        // Permission matrix
        const permTbody = $("#permTbody");
        if (permTbody) {
          const roles = ["Super Admin", "Zone Admin", "Reviewer", "Finance Officer", "Compliance Officer", "Read Only"];
          const modules = [
            { key: "dashboard", label: "Dashboard" },
            { key: "registry", label: "Business Registry" },
            { key: "applications", label: "Applications & Approvals" },
            { key: "licenses", label: "License Management" },
            { key: "investors", label: "Investor Profiles" },
            { key: "infrastructure", label: "Zone Infrastructure" },
            { key: "fees", label: "Fees & Invoicing" },
            { key: "compliance", label: "Compliance & Audits" },
            { key: "analytics", label: "Analytics & Reports" },
            { key: "settings", label: "System Settings" },
            { key: "portal", label: "Portal Configuration" },
            { key: "permissions", label: "Access & Permissions" },
          ];

          function isPermitted(role, modKey) {
            if (role === "Super Admin") return true;
            if (role === "Read Only") return !["settings", "permissions"].includes(modKey);
            if (role === "Zone Admin") return ["dashboard", "registry", "infrastructure", "portal", "permissions", "settings"].includes(modKey);
            if (role === "Reviewer") return ["applications", "registry", "compliance", "dashboard"].includes(modKey);
            if (role === "Finance Officer") return ["fees", "licenses", "invoices", "registry", "dashboard"].includes(modKey);
            if (role === "Compliance Officer") return ["compliance", "registry", "analytics", "dashboard"].includes(modKey);
            return false;
          }

          permTbody.innerHTML = "";
          modules.forEach((m) => {
            const tr = document.createElement("tr");
            const cells = roles
              .map((r) => {
                const allowed = isPermitted(r, m.key);
                return `<td><input type="checkbox" ${allowed ? "checked" : ""} data-perm-role="${r}" data-perm-module="${m.key}" /></td>`;
              })
              .join("");

            tr.innerHTML = `<td style="min-width:180px;">${m.label}</td>${cells}`;
            permTbody.appendChild(tr);
          });
        }

        $("#permTbody")?.addEventListener("change", (e) => {
          const t = e.target;
          if (!(t && t.matches && t.matches("input[type='checkbox'][data-perm-module]"))) return;
          const mod = t.dataset.permModule;
          const role = t.dataset.permRole;
          showToast("Permission updated (mock)", `${role} • ${mod} => ${t.checked ? "ON" : "OFF"}`, "gold");
        });
      }

      renderAccessPermissions();

      // ---------------------------
      // Switches (mock toggles)
      // ---------------------------
      function bindSwitches() {
        $$("input[type='checkbox'][data-switch-key]").forEach((el) => {
          const key = el.dataset.switchKey;
          el.addEventListener("change", () => {
            showToast("Toggle updated", `${key}: ${el.checked ? "ON" : "OFF"}`, el.checked ? "gold" : "blue");
          });
        });
      }
      bindSwitches();

      // ---------------------------
      // Fix: KPIs fade on load already done via initKpis
      // ---------------------------
      // ---------------------------
      // Placeholder: register new business and others show toasts
      // ---------------------------
    </script>
  </body>
</html>
