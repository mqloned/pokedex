<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pokédex Search</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #1a1a1a 0%, #2d2d2d 100%);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            padding: 0;
            transition: background 0.6s ease;
        }

        .auth-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #1a1a1a 0%, #2d2d2d 100%);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }

        .auth-screen.hidden {
            display: none;
        }

        .auth-container {
            background: #262626;
            border: 2px solid #e74c3c;
            border-radius: 12px;
            padding: 40px;
            width: 90%;
            max-width: 400px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.7);
        }

        .auth-tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
            border-bottom: 2px solid #444;
        }

        .auth-tab-btn {
            flex: 1;
            padding: 12px;
            background: transparent;
            border: none;
            color: #888;
            cursor: pointer;
            font-weight: 600;
            font-size: 1rem;
            border-bottom: 3px solid transparent;
            transition: all 0.3s;
        }

        .auth-tab-btn.active {
            color: #e74c3c;
            border-bottom-color: #e74c3c;
        }

        .auth-tab-btn:hover {
            color: #ccc;
        }

        .auth-form {
            display: none;
        }

        .auth-form.active {
            display: block;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-group label {
            display: block;
            margin-bottom: 8px;
            color: #ccc;
            font-weight: 500;
        }

        .form-group input {
            width: 100%;
            padding: 12px;
            background: #1a1a1a;
            border: 2px solid #444;
            border-radius: 6px;
            color: #fff;
            font-size: 1rem;
            transition: border-color 0.3s;
        }

        .form-group input:focus {
            outline: none;
            border-color: #e74c3c;
            box-shadow: 0 0 8px rgba(231, 76, 60, 0.3);
        }

        .auth-submit-btn {
            width: 100%;
            padding: 12px;
            background: #e74c3c;
            color: white;
            border: none;
            border-radius: 6px;
            font-weight: 600;
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.3s;
        }

        .auth-submit-btn:hover {
            background: #c0392b;
            transform: translateY(-2px);
        }

        .auth-error {
            color: #e74c3c;
            margin-top: 10px;
            font-size: 0.9rem;
            text-align: center;
        }

        .profile-container {
            max-width: 600px;
            margin: 0 auto;
        }

        .profile-section {
            background: #262626;
            border: 2px solid #444;
            border-radius: 12px;
            padding: 30px;
        }

        .profile-section h2 {
            color: #e74c3c;
            margin-bottom: 25px;
            font-size: 1.3rem;
        }

        .profile-section h3 {
            color: #ccc;
            font-size: 1rem;
            margin-bottom: 15px;
            margin-top: 20px;
        }

        .avatar-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(80px, 1fr));
            gap: 12px;
            margin-bottom: 20px;
        }

        .avatar-option {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 12px;
            background: #1a1a1a;
            border: 2px solid #444;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
            text-align: center;
        }

        .avatar-option:hover {
            border-color: #e74c3c;
            background: #2a2a2a;
            transform: translateY(-2px);
        }

        .avatar-option.selected {
            border-color: #4CAF50;
            background: rgba(76, 175, 80, 0.1);
            box-shadow: 0 0 12px rgba(76, 175, 80, 0.3);
        }

        .avatar-sprite {
            font-size: 3rem;
            margin-bottom: 6px;
            display: block;
        }

        .avatar-name {
            font-size: 0.75rem;
            color: #888;
            font-weight: 500;
        }

        .profile-preview {
            background: #1a1a1a;
            border: 2px solid #444;
            border-radius: 8px;
            padding: 20px;
            margin: 20px 0;
            text-align: center;
        }

        .preview-card {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 12px;
        }

        .preview-avatar {
            font-size: 4rem;
            width: 100px;
            height: 100px;
            display: flex;
            align-items: center;
            justify-content: center;
            background: #262626;
            border-radius: 8px;
            border: 2px solid #e74c3c;
        }

        .preview-name {
            font-size: 1.3rem;
            color: #e74c3c;
            font-weight: 600;
        }

        .profile-info {
            background: #1a1a1a;
            border: 1px solid #444;
            border-radius: 8px;
            padding: 15px;
            margin-top: 20px;
        }

        .profile-info p {
            color: #ccc;
            margin-bottom: 8px;
            font-size: 0.95rem;
        }

        .profile-info strong {
            color: #e74c3c;
        }

        .welcome-banner {
            background: linear-gradient(135deg, #e74c3c 0%, #c0392b 100%);
            border-radius: 8px;
            padding: 15px 20px;
            margin-bottom: 20px;
            color: white;
            font-weight: 600;
            text-align: center;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            animation: slideDown 0.5s ease-out;
        }

        @keyframes slideDown {
            from {
                opacity: 0;
                transform: translateY(-10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* ============================================================
           TEAM BUILDER STYLES
           ============================================================ */

        .team-container {
            max-width: 1200px;
            margin: 0 auto;
        }

        .team-section,
        .team-stats-section,
        .type-coverage-section,
        .team-selection-section {
            background: #262626;
            border: 2px solid #444;
            border-radius: 12px;
            padding: 25px;
            margin-bottom: 30px;
        }

        .team-section h2,
        .team-stats-section h2,
        .type-coverage-section h2,
        .team-selection-section h2 {
            color: #e74c3c;
            margin-bottom: 15px;
            font-size: 1.3rem;
        }

        .section-subtitle {
            color: #888;
            font-size: 0.9rem;
            margin-bottom: 20px;
        }

        .team-roster {
            display: flex;
            gap: 15px;
            flex-wrap: wrap;
            min-height: 140px;
            align-items: flex-start;
            padding: 15px;
            background: #1a1a1a;
            border-radius: 8px;
            border: 1px solid #444;
        }

        .team-slot {
            flex: 0 0 calc(16.666% - 12px);
            min-width: 110px;
            background: #2a2a2a;
            border: 2px dashed #555;
            border-radius: 8px;
            padding: 10px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            position: relative;
        }

        .team-slot:hover {
            border-color: #e74c3c;
            background: #313131;
        }

        .team-slot.filled {
            border-style: solid;
            border-color: #e74c3c;
            background: #262626;
        }

        .team-slot-content {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 8px;
            height: 100%;
        }

        .team-slot-sprite {
            font-size: 3rem;
            width: 60px;
            height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .team-slot-name {
            font-size: 0.8rem;
            color: #e74c3c;
            font-weight: 600;
            word-break: break-word;
        }

        .team-slot-types {
            display: flex;
            gap: 4px;
            flex-wrap: wrap;
            justify-content: center;
        }

        .team-slot-type-tag {
            font-size: 0.65rem;
            padding: 3px 6px;
            border-radius: 3px;
            background: #444;
            color: #fff;
        }

        .team-slot-remove {
            position: absolute;
            top: 5px;
            right: 5px;
            background: #e74c3c;
            color: white;
            border: none;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            font-size: 0.9rem;
            cursor: pointer;
            transition: all 0.2s;
            display: none;
        }

        .team-slot.filled:hover .team-slot-remove {
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .team-slot-remove:hover {
            background: #c0392b;
            transform: scale(1.1);
        }

        .team-slot-empty {
            color: #666;
            font-size: 0.9rem;
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100%;
        }

        .team-stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
            gap: 15px;
            margin-bottom: 10px;
        }

        .stat-card {
            background: #1a1a1a;
            border: 1px solid #444;
            border-radius: 8px;
            padding: 15px;
            text-align: center;
        }

        .stat-label {
            color: #888;
            font-size: 0.8rem;
            font-weight: 600;
            margin-bottom: 8px;
            text-transform: uppercase;
        }

        .stat-value {
            color: #e74c3c;
            font-size: 1.8rem;
            font-weight: 700;
        }

        .type-coverage-section {
            margin-bottom: 30px;
        }

        .coverage-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }

        .coverage-box {
            background: #1a1a1a;
            border: 1px solid #444;
            border-radius: 8px;
            padding: 20px;
        }

        .coverage-box h3 {
            color: #e74c3c;
            font-size: 1rem;
            margin-bottom: 15px;
        }

        .coverage-types {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
        }

        .coverage-type-tag {
            background: #e74c3c;
            color: white;
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 600;
            text-transform: capitalize;
        }

        .empty-coverage {
            color: #666;
            font-style: italic;
        }

        .favorites-selection {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(130px, 1fr));
            gap: 15px;
            max-height: 400px;
            overflow-y: auto;
            padding: 15px;
            background: #1a1a1a;
            border-radius: 8px;
            border: 1px solid #444;
        }

        .favorite-for-team {
            background: #2a2a2a;
            border: 2px solid #444;
            border-radius: 8px;
            padding: 12px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 8px;
        }

        .favorite-for-team:hover {
            border-color: #e74c3c;
            background: #313131;
        }

        .favorite-for-team.in-team {
            border-color: #4CAF50;
            background: rgba(76, 175, 80, 0.1);
        }

        .favorite-for-team-sprite {
            font-size: 2.5rem;
        }

        .favorite-for-team-name {
            font-size: 0.85rem;
            color: #ccc;
            font-weight: 600;
        }

        .favorite-for-team-added {
            font-size: 0.7rem;
            color: #4CAF50;
            font-weight: 600;
        }

        .nav-bar {
            background: #0f0f0f;
            border-bottom: 2px solid #e74c3c;
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: sticky;
            top: 0;
            z-index: 100;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.5);
        }

        .nav-links {
            display: flex;
            gap: 30px;
        }

        .nav-link {
            color: #ccc;
            cursor: pointer;
            font-weight: 600;
            font-size: 1rem;
            padding: 8px 16px;
            border-radius: 6px;
            transition: all 0.3s;
            border: 2px solid transparent;
            background: none;
        }

        .nav-link:hover {
            color: #e74c3c;
            border-bottom-color: #e74c3c;
        }

        .nav-link.active {
            color: #fff;
            background: #e74c3c;
            border-color: #e74c3c;
        }

        .nav-user-info {
            display: flex;
            align-items: center;
            gap: 15px;
            color: #ccc;
            font-size: 0.95rem;
        }

        .user-display {
            white-space: nowrap;
        }

        .logout-btn {
            padding: 8px 16px;
            background: #e74c3c;
            color: white;
            border: none;
            border-radius: 6px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            font-size: 0.9rem;
        }

        .logout-btn:hover {
            background: #c0392b;
            transform: translateY(-2px);
        }

        .views-container {
            flex: 1;
            display: flex;
            justify-content: center;
            align-items: flex-start;
            padding: 20px;
            overflow-y: auto;
        }

        .view {
            display: none;
            width: 100%;
            max-width: 480px;
        }

        .view.active {
            display: block;
        }

        body.type-fire { background: linear-gradient(135deg, #1a0f0a 0%, #3d1f15 100%); }
        body.type-water { background: linear-gradient(135deg, #0a1a2e 0%, #16364d 100%); }
        body.type-grass { background: linear-gradient(135deg, #132413 0%, #2d5a2d 100%); }
        body.type-electric { background: linear-gradient(135deg, #2a2411 0%, #544a1f 100%); }
        body.type-normal { background: linear-gradient(135deg, #1a1a1a 0%, #2d2d2d 100%); }
        body.type-psychic { background: linear-gradient(135deg, #2a1a29 0%, #4d2a4c 100%); }
        body.type-dragon { background: linear-gradient(135deg, #1a1a2e 0%, #3d2847 100%); }
        body.type-poison { background: linear-gradient(135deg, #2a1a2e 0%, #4d2a54 100%); }

        main {
            width: 100%;
            background: #0f0f0f;
            border-radius: 16px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.8);
            padding: 40px 30px;
            border: 2px solid #333;
        }

        h1 {
            font-size: 3rem;
            color: #e74c3c;
            text-align: center;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
            font-weight: 700;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
        }

        h1 .pokeball-emoji {
            font-size: 3rem;
            line-height: 1;
            display: inline-block;
        }

        .subtitle {
            text-align: center;
            color: #bbb;
            font-size: 0.95rem;
            margin-bottom: 30px;
            line-height: 1.4;
        }

        form {
            margin-bottom: 30px;
        }

        .search-container {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            justify-content: center;
            margin-bottom: 20px;
        }

        input[type="text"] {
            flex: 1;
            min-width: 200px;
            padding: 12px 16px;
            font-size: 1rem;
            border: 2px solid #444;
            border-radius: 8px;
            background: #1a1a1a;
            color: #fff;
            transition: border-color 0.3s, box-shadow 0.3s;
        }

        input[type="text"]:focus {
            outline: none;
            border-color: #e74c3c;
            box-shadow: 0 0 8px rgba(231, 76, 60, 0.3);
            background: #222;
        }

        input[type="text"]::placeholder {
            color: #666;
            animation: placeholderBlink 1.5s ease-in-out infinite;
        }

        @keyframes placeholderBlink {
            0%, 49%, 100% {
                opacity: 1;
            }
            50%, 99% {
                opacity: 0.3;
            }
        }

        @keyframes buttonGlow {
            0% {
                box-shadow: 0 8px 20px rgba(231, 76, 60, 0.4), 0 0 20px rgba(231, 76, 60, 0.6), 0 0 40px rgba(231, 76, 60, 0.3);
            }
            100% {
                box-shadow: 0 8px 20px rgba(231, 76, 60, 0.4), 0 0 30px rgba(231, 76, 60, 0.8), 0 0 60px rgba(231, 76, 60, 0.4);
            }
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-8px); }
            75% { transform: translateX(8px); }
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(15px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        input[type="text"].shake {
            animation: shake 0.5s ease-in-out;
        }

        .pokemon-card {
            animation: fadeInUp 0.5s ease-out;
        }

        .loading-spinner {
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 30px;
        }

        .spinner {
            width: 40px;
            height: 40px;
            border: 4px solid #444;
            border-top: 4px solid #e74c3c;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        button {
            padding: 12px 28px;
            font-size: 1rem;
            background: #e74c3c;
            color: #fff;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            transition: background 0.3s, transform 0.1s, box-shadow 0.3s;
            white-space: nowrap;
        }

        button:hover {
            background: #c0392b;
            box-shadow: 0 8px 20px rgba(231, 76, 60, 0.4), 0 0 20px rgba(231, 76, 60, 0.6), 0 0 40px rgba(231, 76, 60, 0.3);
            transform: translateY(-2px);
            animation: buttonGlow 0.3s ease-out;
        }

        button:active {
            transform: translateY(0);
            box-shadow: 0 4px 12px rgba(231, 76, 60, 0.3);
        }

        button:focus {
            outline: 2px solid #fff;
            outline-offset: 2px;
        }

        .results {
            background: #1a1a1a;
            border-radius: 12px;
            padding: 20px;
            border: 1px solid #333;
            min-height: 100px;
        }

        .empty-state {
            color: #888;
            text-align: center;
            padding: 30px 0;
            font-style: italic;
        }

        .search-history {
            background: #0f0f0f;
            border-radius: 12px;
            padding: 15px;
            border: 1px solid #333;
            margin-bottom: 20px;
        }

        .history-label {
            font-size: 0.85rem;
            color: #aaa;
            margin-bottom: 10px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .history-chips {
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
        }

        .history-chip {
            display: inline-block;
            padding: 6px 14px;
            background: #1a1a1a;
            border: 1px solid #444;
            border-radius: 20px;
            font-size: 0.85rem;
            color: #ccc;
            cursor: pointer;
            transition: all 0.3s;
            text-transform: capitalize;
        }

        .history-chip:hover {
            background: #e74c3c;
            color: #fff;
            border-color: #e74c3c;
            transform: translateY(-2px);
        }

        .empty-history {
            color: #666;
            font-size: 0.85rem;
            font-style: italic;
        }

        .controls-bar {
            display: flex;
            gap: 10px;
            justify-content: center;
            align-items: center;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .mode-toggle {
            padding: 8px 16px;
            font-size: 0.9rem;
            background: #333;
            color: #ccc;
            border: 2px solid #555;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: 600;
        }

        .mode-toggle.active {
            background: #e74c3c;
            color: #fff;
            border-color: #e74c3c;
        }

        .mode-toggle:hover {
            border-color: #e74c3c;
        }

        .sound-toggle {
            width: 40px;
            height: 40px;
            padding: 6px;
            background: #333;
            border: 2px solid #555;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1.2rem;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
        }

        .sound-toggle:hover {
            background: #444;
        }

        .sound-toggle.muted {
            opacity: 0.5;
        }

        .compare-inputs {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-bottom: 20px;
        }

        .compare-input-group {
            display: flex;
            gap: 8px;
        }

        .compare-input-group input {
            flex: 1;
            padding: 10px 12px;
            font-size: 0.95rem;
        }

        .compare-results {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }

        .compare-card {
            background: #1a1a1a;
            border-radius: 12px;
            padding: 15px;
            border: 1px solid #333;
            animation: fadeInUp 0.5s ease-out;
        }

        @media (max-width: 900px) {
            .compare-results {
                grid-template-columns: 1fr;
            }
        }

        .favorite-btn {
            position: absolute;
            top: 10px;
            right: 10px;
            width: 40px;
            height: 40px;
            background: rgba(255, 255, 255, 0.1);
            border: 2px solid #e74c3c;
            border-radius: 50%;
            cursor: pointer;
            font-size: 1.3rem;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
            padding: 0;
        }

        .favorite-btn:hover {
            background: rgba(231, 76, 60, 0.2);
            transform: scale(1.1);
        }

        .favorite-btn.favorited {
            background: rgba(231, 76, 60, 0.3);
        }

        .favorites-section {
            background: #0f0f0f;
            border-radius: 12px;
            padding: 20px;
            border: 1px solid #333;
            margin-top: 30px;
        }

        .favorites-title {
            font-size: 1.3rem;
            color: #e74c3c;
            font-weight: 700;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .favorites-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
            gap: 12px;
        }

        .favorite-card {
            background: #1a1a1a;
            border: 1px solid #333;
            border-radius: 8px;
            padding: 12px;
            text-align: center;
            position: relative;
            transition: all 0.3s;
            cursor: pointer;
        }

        .favorite-card:hover {
            border-color: #e74c3c;
            background: #222;
            transform: translateY(-3px);
        }

        .favorite-card img {
            width: 100%;
            max-width: 80px;
            height: auto;
            margin-bottom: 8px;
        }

        .favorite-card-name {
            font-size: 0.85rem;
            color: #ccc;
            font-weight: 600;
            text-transform: capitalize;
            margin-bottom: 8px;
            word-break: break-word;
        }

        .favorite-card-remove {
            position: absolute;
            top: 4px;
            right: 4px;
            width: 24px;
            height: 24px;
            background: #e74c3c;
            color: #fff;
            border: none;
            border-radius: 50%;
            cursor: pointer;
            font-size: 0.9rem;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
            padding: 0;
            line-height: 1;
        }

        .favorite-card-remove:hover {
            background: #c0392b;
            transform: scale(1.15);
        }

        .no-favorites {
            color: #666;
            text-align: center;
            padding: 30px;
            font-style: italic;
            font-size: 0.95rem;
        }

        .favorites-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            flex-wrap: wrap;
            gap: 10px;
        }

        .favorites-counter {
            color: #e74c3c;
            font-weight: 700;
            font-size: 1rem;
        }

        .export-btn {
            padding: 8px 16px;
            font-size: 0.9rem;
            background: #27ae60;
            color: #fff;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: 600;
        }

        .export-btn:hover {
            background: #229954;
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(39, 174, 96, 0.3);
        }

        .export-btn:active {
            transform: translateY(0);
        }

        .export-btn:disabled {
            background: #95a5a6;
            cursor: not-allowed;
            transform: none;
        }

        .pokemon-card {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .pokemon-header {
            display: flex;
            justify-content: space-between;
            align-items: start;
            margin-bottom: 10px;
        }

        .pokemon-name {
            font-size: 1.8rem;
            color: #fff;
            font-weight: 700;
            text-transform: capitalize;
        }

        .pokemon-id {
            color: #e74c3c;
            font-size: 0.9rem;
            font-weight: 600;
        }

        .pokemon-image {
            text-align: center;
            padding: 20px 0;
        }

        .pokemon-image svg,
        .pokemon-image div {
            font-size: 80px;
            line-height: 1;
        }

        .pokemon-types {
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
            justify-content: center;
        }

        .type-tag {
            display: inline-block;
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 600;
            text-transform: capitalize;
            background: #333;
            color: #fff;
            border: 1px solid #555;
        }

        .type-tag.fire {
            background: #f08030;
            border-color: #e67e22;
            color: #fff;
        }

        .type-tag.water {
            background: #6890f0;
            border-color: #3498db;
            color: #fff;
        }

        .type-tag.grass {
            background: #78c850;
            border-color: #27ae60;
            color: #fff;
        }

        .type-tag.electric {
            background: #f8d030;
            border-color: #f39c12;
            color: #000;
        }

        .type-tag.psychic {
            background: #f85888;
            border-color: #e91e63;
            color: #fff;
        }

        .type-tag.dragon {
            background: #7038f8;
            border-color: #6c5ce7;
            color: #fff;
        }

        .type-tag.normal {
            background: #a8a878;
            border-color: #95a5a6;
            color: #fff;
        }

        .type-tag.poison {
            background: #a040a0;
            border-color: #8e44ad;
            color: #fff;
        }

        .pokemon-stats {
            background: #0f0f0f;
            border-radius: 8px;
            padding: 15px;
            border: 1px solid #333;
        }

        .stat-row {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            font-size: 0.9rem;
            color: #ccc;
        }

        .stat-row:last-child {
            margin-bottom: 0;
        }

        .stat-label {
            color: #aaa;
            font-weight: 500;
        }

        .stat-value {
            color: #e74c3c;
            font-weight: 600;
        }

        .pokemon-dimensions {
            background: #0f0f0f;
            border-radius: 8px;
            padding: 15px;
            border: 1px solid #333;
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
        }

        .dimension {
            text-align: center;
        }

        .dimension-label {
            color: #aaa;
            font-size: 0.85rem;
            font-weight: 500;
            margin-bottom: 5px;
        }

        .dimension-value {
            color: #e74c3c;
            font-size: 1.2rem;
            font-weight: 600;
        }

        @media (max-width: 600px) {
            main {
                padding: 30px 20px;
            }

            h1 {
                font-size: 2.5rem;
            }

            .search-container {
                flex-direction: column;
            }

            input[type="text"] {
                width: 100%;
                min-width: unset;
            }

            button {
                width: 100%;
            }

            .pokemon-name {
                font-size: 1.5rem;
            }

            .pokemon-image svg,
            .pokemon-image div {
                font-size: 60px;
            }
        }

        @media (max-width: 600px) {
            .nav-bar {
                flex-direction: column;
                gap: 15px;
            }

            .nav-links {
                width: 100%;
                justify-content: center;
                gap: 15px;
            }

            .nav-user-info {
                width: 100%;
                justify-content: center;
                font-size: 0.85rem;
            }

            .logout-btn {
                padding: 6px 12px;
                font-size: 0.8rem;
            }

            .auth-container {
                margin: 20px;
                padding: 30px 20px;
            }

            .auth-tabs {
                margin-bottom: 20px;
            }

            .auth-tab-btn {
                font-size: 0.9rem;
                padding: 10px;
            }

            .form-group input {
                font-size: 16px; /* Prevents zoom-in on iOS */
            }

            .team-roster {
                gap: 12px;
                min-height: 130px;
            }

            .team-slot {
                flex: 0 0 calc(33.333% - 8px);
                min-width: 90px;
            }

            .team-slot-sprite {
                font-size: 2.5rem;
                width: 55px;
                height: 55px;
            }

            .team-stats-grid {
                grid-template-columns: repeat(3, 1fr);
            }

            .coverage-grid {
                grid-template-columns: 1fr 1fr;
            }

            .favorites-selection {
                grid-template-columns: repeat(3, 1fr);
                gap: 12px;
                padding: 12px;
            }
        }

        @media (max-width: 400px) {
            main {
                padding: 25px 15px;
            }

            h1 {
                font-size: 2rem;
            }

            input[type="text"],
            button {
                font-size: 0.95rem;
                padding: 10px 14px;
            }

            .subtitle {
                font-size: 0.85rem;
            }

            .history-chip {
                padding: 5px 12px;
                font-size: 0.8rem;
            }

            .search-history {
                margin-bottom: 15px;
                padding: 12px;
            }

            .auth-container {
                width: 95%;
                padding: 25px 15px;
            }

            .avatar-grid {
                grid-template-columns: repeat(auto-fit, minmax(70px, 1fr));
                gap: 10px;
            }

            .profile-section {
                padding: 20px 15px;
            }

            .profile-container {
                padding: 0;
            }

            .team-roster {
                gap: 10px;
                min-height: 120px;
            }

            .team-slot {
                flex: 0 0 calc(50% - 5px);
                min-width: 100px;
            }

            .team-slot-sprite {
                font-size: 2rem;
                width: 50px;
                height: 50px;
            }

            .team-slot-name {
                font-size: 0.75rem;
            }

            .team-stats-grid {
                grid-template-columns: repeat(2, 1fr);
                gap: 10px;
            }

            .stat-card {
                padding: 12px;
            }

            .stat-label {
                font-size: 0.7rem;
            }

            .stat-value {
                font-size: 1.5rem;
            }

            .coverage-grid {
                grid-template-columns: 1fr;
                gap: 15px;
            }

            .coverage-box {
                padding: 15px;
            }

            .favorites-selection {
                grid-template-columns: repeat(2, 1fr);
                max-height: 300px;
                gap: 10px;
                padding: 10px;
            }

            .favorite-for-team {
                padding: 10px;
            }

            .favorite-for-team-sprite {
                font-size: 2rem;
            }

            .favorite-for-team-name {
                font-size: 0.75rem;
            }

            .team-section,
            .team-stats-section,
            .type-coverage-section,
            .team-selection-section {
                padding: 15px;
                margin-bottom: 20px;
            }

            .team-section h2,
            .team-stats-section h2,
            .type-coverage-section h2,
            .team-selection-section h2 {
                font-size: 1.1rem;
            }
        }

        .about-content {
            color: #ccc;
            line-height: 1.8;
        }

        .about-content h2 {
            color: #e74c3c;
            font-size: 1.5rem;
            margin-bottom: 15px;
            margin-top: 20px;
        }

        .about-content h2:first-child {
            margin-top: 0;
        }

        .about-content p {
            margin-bottom: 15px;
        }

        .about-content ul {
            margin-left: 20px;
            margin-bottom: 15px;
        }

        .about-content li {
            margin-bottom: 8px;
        }

        .credit-line {
            margin-top: 30px;
            padding-top: 20px;
            border-top: 1px solid #333;
            color: #888;
            font-size: 0.9rem;
            font-style: italic;
        }
    </style>
</head>
<body>
    <!-- AUTH SCREEN REMOVED - No authentication required -->

    <nav class="nav-bar">
        <div class="nav-links">
            <button class="nav-link active" id="navHome" data-view="home">Home</button>
            <button class="nav-link" id="navFavorites" data-view="favorites">Favorites</button>
            <button class="nav-link" id="navTeam" data-view="team">Team Builder</button>
            <button class="nav-link" id="navProfile" data-view="profile">Profile</button>
            <button class="nav-link" id="navAbout" data-view="about">About</button>
        </div>
        <div class="nav-user-info">
            <span id="userDisplay" class="user-display">User: <strong id="usernamDisplay">Pokémon Trainer</strong></span>
            <button type="button" id="logoutBtn" class="logout-btn" aria-label="Logout" style="display: none;">Logout</button>
        </div>
    </nav>

    <div class="views-container">
        <!-- HOME VIEW -->
        <section id="home" class="view active">
            <main>
                <h1><span class="pokeball-emoji">⚪🔴</span>Pokédex</h1>
                <p class="subtitle">Search for a Pokémon by name to discover its details.</p>

                <form id="searchForm" role="search">
                    <div class="search-container">
                        <input
                            type="text"
                            id="searchInput"
                            placeholder="Search Pokémon by name…"
                            aria-label="Search for a Pokémon"
                            autocomplete="off"
                        />
                        <button type="submit" aria-label="Search for Pokémon">Search</button>
                        <button type="button" id="randomBtn" aria-label="Search for a random Pokémon">Random</button>
                    </div>
                </form>

                <div class="controls-bar">
                    <button type="button" id="soundToggle" class="sound-toggle" aria-label="Toggle sound effects">🔊</button>
                    <button type="button" id="compareModeBtn" class="mode-toggle">Compare Mode</button>
                </div>

                <form id="compareForm" style="display: none;">
                    <div class="compare-inputs">
                        <div class="compare-input-group">
                            <input
                                type="text"
                                id="compareInput1"
                                placeholder="Pokémon 1…"
                                aria-label="Search for first Pokémon to compare"
                                autocomplete="off"
                            />
                            <button type="button" id="compareSearch1">Search</button>
                        </div>
                        <div class="compare-input-group">
                            <input
                                type="text"
                                id="compareInput2"
                                placeholder="Pokémon 2…"
                                aria-label="Search for second Pokémon to compare"
                                autocomplete="off"
                            />
                            <button type="button" id="compareSearch2">Search</button>
                        </div>
                    </div>
                </form>

                <section id="historySection" class="search-history">
                    <div class="history-label">Recent Searches</div>
                    <div id="historychips" class="history-chips">
                        <div class="empty-history">No recent searches yet</div>
                    </div>
                </section>

                <section
                    id="results"
                    class="results"
                    aria-live="polite"
                    aria-label="Pokémon details"
                >
                    <div class="empty-state">Search for a Pokémon to see details.</div>
                </section>

                <section id="favoritesSection" class="favorites-section" style="display: none;">
                    <div class="favorites-header">
                        <h2 class="favorites-title">⭐ My Favorites <span id="favoritesCounter" class="favorites-counter">(0)</span></h2>
                        <button type="button" id="exportBtn" class="export-btn" aria-label="Download favorites as JSON">Download JSON</button>
                    </div>
                    <div id="favoritesGrid" class="favorites-grid">
                        <div class="no-favorites">No favorite Pokémon yet</div>
                    </div>
                </section>
            </main>
        </section>

        <!-- FAVORITES VIEW -->
        <section id="favorites" class="view">
            <main>
                <h1 style="margin-bottom: 30px;">⭐ My Favorites</h1>
                <div id="favoritesViewGrid" class="favorites-grid" style="min-height: 200px;">
                    <div class="no-favorites">No favorite Pokémon yet</div>
                </div>
            </main>
        </section>

        <!-- PROFILE VIEW -->
        <section id="profile" class="view">
            <main>
                <h1 style="margin-bottom: 30px;">👤 My Profile</h1>
                <div class="profile-container">
                    <div class="profile-section">
                        <h2>Profile Settings</h2>
                        
                        <div class="form-group">
                            <label for="displayName">Display Name</label>
                            <input type="text" id="displayName" placeholder="Enter your display name" maxlength="30">
                        </div>

                        <div class="form-group">
                            <label>Choose Avatar</label>
                            <div class="avatar-grid" id="avatarGrid">
                                <!-- Avatar options will be populated by JavaScript -->
                            </div>
                        </div>

                        <div class="profile-preview">
                            <h3>Preview</h3>
                            <div class="preview-card">
                                <div class="preview-avatar" id="previewAvatar">⚪</div>
                                <div class="preview-name" id="previewName">Your Display Name</div>
                            </div>
                        </div>

                        <button type="button" id="saveProfileBtn" class="auth-submit-btn">Save Profile</button>
                        <div id="profileMessage" class="auth-error" style="margin-top: 15px;"></div>

                        <div class="profile-info">
                            <h3>Account Information</h3>
                            <p><strong>Username:</strong> <span id="accountUsername">—</span></p>
                            <p><strong>Created:</strong> <span id="accountCreated">—</span></p>
                            <p><strong>Favorites:</strong> <span id="accountFavorites">0</span></p>
                        </div>
                    </div>
                </div>
            </main>
        </section>

        <!-- TEAM BUILDER VIEW -->
        <section id="team" class="view">
            <main>
                <h1 style="margin-bottom: 30px;">🏆 Team Builder</h1>
                <div class="team-container">
                    <!-- Current Team Display -->
                    <div class="team-section">
                        <h2>Your Team</h2>
                        <p class="section-subtitle">Select up to 6 Pokémon from your favorites</p>
                        <div class="team-roster" id="teamRoster">
                            <!-- Team slots will be populated by JavaScript -->
                        </div>
                    </div>

                    <!-- Team Stats -->
                    <div class="team-stats-section">
                        <h2>Combined Stats</h2>
                        <div class="team-stats-grid" id="teamStatsGrid">
                            <div class="stat-card">
                                <div class="stat-label">Total HP</div>
                                <div class="stat-value" id="statTotalHP">0</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-label">Avg HP</div>
                                <div class="stat-value" id="statAvgHP">0</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-label">Total ATK</div>
                                <div class="stat-value" id="statTotalATK">0</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-label">Avg ATK</div>
                                <div class="stat-value" id="statAvgATK">0</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-label">Total DEF</div>
                                <div class="stat-value" id="statTotalDEF">0</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-label">Avg DEF</div>
                                <div class="stat-value" id="statAvgDEF">0</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-label">Total SP.ATK</div>
                                <div class="stat-value" id="statTotalSPA">0</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-label">Avg SP.ATK</div>
                                <div class="stat-value" id="statAvgSPA">0</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-label">Total SP.DEF</div>
                                <div class="stat-value" id="statTotalSPD">0</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-label">Avg SP.DEF</div>
                                <div class="stat-value" id="statAvgSPD">0</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-label">Total SPD</div>
                                <div class="stat-value" id="statTotalSpeed">0</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-label">Avg SPD</div>
                                <div class="stat-value" id="statAvgSpeed">0</div>
                            </div>
                        </div>
                    </div>

                    <!-- Type Coverage -->
                    <div class="type-coverage-section">
                        <h2>Type Coverage</h2>
                        <div class="coverage-grid">
                            <div class="coverage-box">
                                <h3>Strong Against</h3>
                                <div class="coverage-types" id="strongAgainst">
                                    <span class="empty-coverage">No team selected</span>
                                </div>
                            </div>
                            <div class="coverage-box">
                                <h3>Weak To</h3>
                                <div class="coverage-types" id="weakTo">
                                    <span class="empty-coverage">No team selected</span>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Favorites Selection -->
                    <div class="team-selection-section">
                        <h2>Select from Favorites</h2>
                        <div class="favorites-selection" id="favoritesSelection">
                            <div class="no-favorites">No favorite Pokémon yet. Add some from the Home page!</div>
                        </div>
                    </div>
                </div>
            </main>
        </section>

        <!-- ABOUT VIEW -->
        <section id="about" class="view">
            <main>
                <h1 style="margin-bottom: 30px;">About Pokédex</h1>
                <div class="about-content">
                    <h2>What is this?</h2>
                    <p>This is a modern, interactive Pokédex web application built with vanilla HTML, CSS, and JavaScript. It allows you to search, browse, and manage your favorite Pokémon all in one place.</p>

                    <h2>Features</h2>
                    <ul>
                        <li><strong>Search</strong> - Find Pokémon by name and view detailed stats</li>
                        <li><strong>Browse</strong> - See Pokémon sprites, types, and base stats</li>
                        <li><strong>Random</strong> - Discover a random Pokémon with one click</li>
                        <li><strong>Compare</strong> - Side-by-side comparison of two Pokémon</li>
                        <li><strong>Favorites</strong> - Save your favorite Pokémon to your personal collection</li>
                        <li><strong>Export</strong> - Download your favorites as a JSON file</li>
                        <li><strong>Dynamic Themes</strong> - Background color changes based on Pokémon type</li>
                        <li><strong>Sound Effects</strong> - Optional audio feedback (toggleable)</li>
                        <li><strong>Search History</strong> - Quick access to recently viewed Pokémon</li>
                    </ul>

                    <h2>Data Source</h2>
                    <p>All Pokémon data, including names, sprites, types, and stats, is provided by the <strong>PokéAPI</strong> - a free, open-source API.</p>

                    <div class="credit-line">
                        <strong>Data Source:</strong> <a href="https://pokeapi.co/" target="_blank" style="color: #e74c3c; text-decoration: none;">PokéAPI.co</a> - A free API for Pokémon data<br>
                        <strong>Built with:</strong> Vanilla HTML, CSS, and JavaScript | <strong>Storage:</strong> IndexedDB for persistence
                    </div>
                </div>
            </main>
        </section>
    </div>

    <script>
        const searchForm = document.getElementById('searchForm');
        const searchInput = document.getElementById('searchInput');
        const resultsSection = document.getElementById('results');
        const randomBtn = document.getElementById('randomBtn');
        const soundToggle = document.getElementById('soundToggle');
        const compareModeBtn = document.getElementById('compareModeBtn');
        const compareForm = document.getElementById('compareForm');
        const compareInput1 = document.getElementById('compareInput1');
        const compareInput2 = document.getElementById('compareInput2');
        const compareSearch1 = document.getElementById('compareSearch1');
        const compareSearch2 = document.getElementById('compareSearch2');
        const historyChipsContainer = document.getElementById('historychips');
        const favoritesSection = document.getElementById('favoritesSection');
        const favoritesGrid = document.getElementById('favoritesGrid');
        const favoritesViewGrid = document.getElementById('favoritesViewGrid');
        const favoritesCounter = document.getElementById('favoritesCounter');
        const exportBtn = document.getElementById('exportBtn');
        const navLinks = document.querySelectorAll('.nav-link');
        const views = document.querySelectorAll('.view');
        const authTabBtns = document.querySelectorAll('.auth-tab-btn');
        const usernameDisplay = document.getElementById('usernamDisplay');
        const API_BASE_URL = 'https://pokeapi.co/api/v2/pokemon';
        const MAX_POKEMON_ID = 1010;
        const MAX_HISTORY = 5;
        let searchHistory = JSON.parse(localStorage.getItem('pokemonHistory')) || [];
        let isSoundEnabled = JSON.parse(localStorage.getItem('soundEnabled')) !== false;
        let isCompareMode = false;
        let comparePokemon = [null, null];
        let favorites = [];
        let db = null;
        let dbReady = false;
        let currentView = 'home';
        let currentUser = null;

        // ============================================================
        // AUTHENTICATION SYSTEM - User Management & Login/Signup
        // ============================================================

        /**
         * Simple password hashing using SubtleCrypto SHA-256
         * Not for production use - for demo purposes only
         */
        async function hashPassword(password) {
            const encoder = new TextEncoder();
            const data = encoder.encode(password);
            const hashBuffer = await crypto.subtle.digest('SHA-256', data);
            const hashArray = Array.from(new Uint8Array(hashBuffer));
            return hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
        }

        /**
         * Check if a user exists in localStorage
         */
        function userExists(username) {
            return localStorage.getItem(`user_${username}`) !== null;
        }

        // AUTHENTICATION FUNCTIONS REMOVED - No login/password required

        /**
         * Update UI to show logged-in user
         */
        function updateUserDisplay() {
            if (currentUser) {
                const displayName = getDisplayName(currentUser);
                usernameDisplay.textContent = displayName;
            } else {
                usernameDisplay.textContent = 'Pokémon Trainer';
            }
        }

        /**
         * Initialize authentication on page load
         */
        function initializeAuth() {
            // Auto-login as guest user (no authentication required)
            const guestUser = 'guest';
            
            // Create guest profile if it doesn't exist
            const existingProfile = localStorage.getItem(`profile_${guestUser}`);
            if (!existingProfile) {
                const guestProfile = {
                    displayName: "Pokémon Trainer",
                    avatar: {
                        id: 'pikachu',
                        name: 'Pikachu',
                        emoji: '⚡'
                    },
                    updatedAt: new Date().toISOString()
                };
                localStorage.setItem(`profile_${guestUser}`, JSON.stringify(guestProfile));
            }
            
            // Set current user to guest
            currentUser = guestUser;
            document.body.style.overflow = 'auto';
            updateUserDisplay();
        }

        // ============================================================
        // PROFILE MANAGEMENT - User Display Name & Avatar Selection
        // ============================================================

        /**
         * Predefined avatar list (Pokémon sprites)
         */
        const AVATAR_OPTIONS = [
            { id: 'pikachu', name: 'Pikachu', emoji: '⚡' },
            { id: 'charmander', name: 'Charmander', emoji: '🔥' },
            { id: 'squirtle', name: 'Squirtle', emoji: '💧' },
            { id: 'bulbasaur', name: 'Bulbasaur', emoji: '🌿' },
            { id: 'psyduck', name: 'Psyduck', emoji: '🦆' },
            { id: 'machop', name: 'Machop', emoji: '💪' },
            { id: 'drowzee', name: 'Drowzee', emoji: '😴' },
            { id: 'shellder', name: 'Shellder', emoji: '🐚' },
            { id: 'exeggcute', name: 'Exeggcute', emoji: '🥚' },
            { id: 'staryu', name: 'Staryu', emoji: '⭐' },
            { id: 'magikarp', name: 'Magikarp', emoji: '🐟' },
            { id: 'lapras', name: 'Lapras', emoji: '🐉' }
        ];

        let selectedAvatar = null;

        /**
         * Get user profile from localStorage
         */
        function getUserProfile(username) {
            const profile = localStorage.getItem(`profile_${username}`);
            return profile ? JSON.parse(profile) : null;
        }

        /**
         * Save user profile to localStorage
         */
        function saveUserProfile(username, profile) {
            localStorage.setItem(`profile_${username}`, JSON.stringify(profile));
        }

        /**
         * Get profile display name (fallback to username if not set)
         */
        function getDisplayName(username) {
            const profile = getUserProfile(username);
            return profile?.displayName || username;
        }

        /**
         * Get user's selected avatar
         */
        function getUserAvatar(username) {
            const profile = getUserProfile(username);
            return profile?.avatar || AVATAR_OPTIONS[0];
        }

        /**
         * Initialize Profile page with current data
         */
        function initializeProfilePage() {
            if (!currentUser) return;

            const profile = getUserProfile(currentUser);
            const userDataElement = localStorage.getItem(`user_${currentUser}`);
            const userData = userDataElement ? JSON.parse(userDataElement) : null;

            // Set display name input
            const displayNameInput = document.getElementById('displayName');
            displayNameInput.value = profile?.displayName || '';

            // Pop account info
            document.getElementById('accountUsername').textContent = currentUser;
            document.getElementById('accountCreated').textContent = userData?.createdAt 
                ? new Date(userData.createdAt).toLocaleDateString() 
                : 'Unknown';
            document.getElementById('accountFavorites').textContent = favorites.length;

            // Render avatar grid
            renderAvatarGrid(profile?.avatar?.id);

            // Update preview
            updateProfilePreview(profile?.displayName || '', profile?.avatar || AVATAR_OPTIONS[0]);
        }

        /**
         * Render avatar selection grid
         */
        function renderAvatarGrid(selectedAvatarId) {
            const avatarGrid = document.getElementById('avatarGrid');
            avatarGrid.innerHTML = AVATAR_OPTIONS.map(avatar => `
                <div class="avatar-option ${selectedAvatarId === avatar.id ? 'selected' : ''}" data-avatar-id="${avatar.id}">
                    <span class="avatar-sprite">${avatar.emoji}</span>
                    <span class="avatar-name">${avatar.name}</span>
                </div>
            `).join('');

            // Add click handlers
            avatarGrid.querySelectorAll('.avatar-option').forEach(option => {
                option.addEventListener('click', () => {
                    const avatarId = option.getAttribute('data-avatar-id');
                    selectedAvatar = AVATAR_OPTIONS.find(a => a.id === avatarId);

                    // Update selected class
                    avatarGrid.querySelectorAll('.avatar-option').forEach(opt => {
                        opt.classList.remove('selected');
                    });
                    option.classList.add('selected');

                    // Update preview
                    const displayName = document.getElementById('displayName').value || '';
                    updateProfilePreview(displayName, selectedAvatar);
                });
            });

            // Set selected avatar from profile or default
            selectedAvatar = AVATAR_OPTIONS.find(a => a.id === selectedAvatarId) || AVATAR_OPTIONS[0];
        }

        /**
         * Update profile preview display
         */
        function updateProfilePreview(displayName, avatar) {
            document.getElementById('previewName').textContent = displayName || 'Your Display Name';
            document.getElementById('previewAvatar').textContent = avatar?.emoji || '⚪';
        }

        /**
         * Save profile changes
         */
        function saveProfile() {
            if (!currentUser) {
                alert('User not logged in!');
                return;
            }

            const displayName = document.getElementById('displayName').value.trim();
            const avatar = selectedAvatar || AVATAR_OPTIONS[0];

            if (!displayName) {
                alert('Please enter a display name!');
                return;
            }

            const profile = {
                displayName: displayName,
                avatar: avatar,
                updatedAt: new Date().toISOString()
            };

            saveUserProfile(currentUser, profile);
            updateUserDisplay();

            // Show success message
            const messageDiv = document.getElementById('profileMessage');
            messageDiv.style.color = '#4CAF50';
            messageDiv.textContent = 'Profile saved successfully!';
            setTimeout(() => {
                messageDiv.textContent = '';
            }, 3000);
        }

        /**
         * Show welcome back message with user's display name
         */
        function showWelcomeMessage() {
            if (!currentUser) return;

            const displayName = getDisplayName(currentUser);
            const avatar = getUserAvatar(currentUser);

            // Check if welcome message already exists
            let welcomeBanner = document.getElementById('welcomeBanner');
            if (welcomeBanner) {
                welcomeBanner.remove();
            }

            // Create and insert welcome banner
            const banner = document.createElement('div');
            banner.id = 'welcomeBanner';
            banner.className = 'welcome-banner';
            banner.innerHTML = `${avatar.emoji} Welcome back, <strong>${displayName}</strong>!`;

            // Insert at the top of the results section or main content
            const mainElement = document.querySelector('main');
            if (mainElement) {
                mainElement.insertBefore(banner, mainElement.firstChild);
                
                // Auto-remove after 5 seconds
                setTimeout(() => {
                    if (welcomeBanner) {
                        welcomeBanner.remove();
                    }
                }, 5000);
            }
        }

        // ============================================================
        // TEAM BUILDER SYSTEM - Team Management & Type Coverage
        // ============================================================

        /**
         * Pokémon type matchup data
         * Indicates which types each type is effective/weak against
         */
        const TYPE_MATCHUPS = {
            normal: { strongAgainst: [], weakTo: ['rock', 'ghost'] },
            fire: { strongAgainst: ['grass', 'bug', 'steel'], weakTo: ['water', 'ground', 'rock'] },
            water: { strongAgainst: ['fire', 'ground', 'rock'], weakTo: ['grass', 'electric'] },
            grass: { strongAgainst: ['water', 'ground', 'rock'], weakTo: ['fire', 'bug', 'poison', 'flying'] },
            electric: { strongAgainst: ['water', 'flying'], weakTo: ['ground'] },
            ice: { strongAgainst: ['grass', 'flying', 'ground', 'dragon'], weakTo: ['fire', 'fighting', 'rock', 'steel'] },
            fighting: { strongAgainst: ['normal', 'rock', 'steel', 'ice', 'dark'], weakTo: ['flying', 'psychic', 'fairy'] },
            poison: { strongAgainst: ['grass', 'fairy'], weakTo: ['ground', 'psychic'] },
            ground: { strongAgainst: ['fire', 'electric', 'poison', 'rock', 'steel'], weakTo: ['water', 'grass', 'ice'] },
            flying: { strongAgainst: ['grass', 'fighting', 'bug'], weakTo: ['electric', 'ice', 'rock'] },
            psychic: { strongAgainst: ['fighting', 'poison'], weakTo: ['bug', 'ghost', 'dark'] },
            bug: { strongAgainst: ['grass', 'psychic', 'dark'], weakTo: ['fire', 'flying', 'rock'] },
            rock: { strongAgainst: ['flying', 'bug', 'fire', 'ice'], weakTo: ['water', 'grass', 'fighting', 'ground', 'steel'] },
            ghost: { strongAgainst: ['ghost', 'psychic'], weakTo: ['ghost', 'dark'] },
            dragon: { strongAgainst: ['dragon'], weakTo: ['ice', 'dragon', 'fairy'] },
            dark: { strongAgainst: ['ghost', 'psychic'], weakTo: ['fighting', 'bug', 'fairy'] },
            steel: { strongAgainst: ['normal', 'flying', 'rock', 'fairy', 'ice'], weakTo: ['fire', 'water', 'ground'] },
            fairy: { strongAgainst: ['fighting', 'dragon', 'dark'], weakTo: ['poison', 'steel'] }
        };

        let userTeam = [];

        /**
         * Load user's team from localStorage
         */
        function loadUserTeam() {
            if (!currentUser) return;
            const savedTeam = localStorage.getItem(`team_${currentUser}`);
            userTeam = savedTeam ? JSON.parse(savedTeam) : [];
        }

        /**
         * Save user's team to localStorage
         */
        function saveUserTeam() {
            if (!currentUser) return;
            localStorage.setItem(`team_${currentUser}`, JSON.stringify(userTeam));
        }

        /**
         * Add Pokémon to team (max 6)
         */
        function addToTeam(pokemonName) {
            const pokemon = favorites.find(fav => fav.name.toLowerCase() === pokemonName.toLowerCase());
            
            if (!pokemon) {
                alert('Pokémon not found in favorites!');
                return;
            }

            if (userTeam.length >= 6) {
                alert('Team is full! Remove a Pokémon first.');
                return;
            }

            if (userTeam.some(p => p.name.toLowerCase() === pokemonName.toLowerCase())) {
                alert('Pokémon is already in your team!');
                return;
            }

            userTeam.push(pokemon);
            saveUserTeam();
            updateTeamDisplay();
        }

        /**
         * Remove Pokémon from team by index
         */
        function removeFromTeam(index) {
            userTeam.splice(index, 1);
            saveUserTeam();
            updateTeamDisplay();
        }

        /**
         * Calculate combined stats for the team
         */
        function calculateTeamStats() {
            const stats = {
                totalHP: 0,
                totalATK: 0,
                totalDEF: 0,
                totalSPA: 0,
                totalSPD: 0,
                totalSpeed: 0
            };

            userTeam.forEach(pokemon => {
                // Find full pokemon data from favorites (which has stats from API)
                const fullData = favorites.find(f => f.name.toLowerCase() === pokemon.name.toLowerCase());
                if (fullData && fullData.stats) {
                    const statMap = {};
                    fullData.stats.forEach(stat => {
                        statMap[stat.stat.name] = stat.base_stat;
                    });
                    stats.totalHP += statMap['hp'] || 0;
                    stats.totalATK += statMap['attack'] || 0;
                    stats.totalDEF += statMap['defense'] || 0;
                    stats.totalSPA += statMap['spa'] || 0;
                    stats.totalSPD += statMap['spd'] || 0;
                    stats.totalSpeed += statMap['speed'] || 0;
                }
            });

            return stats;
        }

        /**
         * Calculate type coverage (strong against and weak to)
         */
        function calculateTypeCoverage() {
            const coverage = {
                strongAgainst: new Set(),
                weakTo: new Set()
            };

            // Collect all types in the team
            const teamTypes = new Set();
            userTeam.forEach(pokemon => {
                if (pokemon.types) {
                    pokemon.types.forEach(type => {
                        teamTypes.add(type);
                    });
                }
            });

            // Calculate what the team is strong against and weak to
            teamTypes.forEach(type => {
                const matchup = TYPE_MATCHUPS[type.toLowerCase()];
                if (matchup) {
                    matchup.strongAgainst.forEach(t => coverage.strongAgainst.add(t));
                    matchup.weakTo.forEach(t => coverage.weakTo.add(t));
                }
            });

            return {
                strongAgainst: Array.from(coverage.strongAgainst).sort(),
                weakTo: Array.from(coverage.weakTo).sort()
            };
        }

        /**
         * Update team display and stats
         */
        function updateTeamDisplay() {
            // Update team roster
            const teamRoster = document.getElementById('teamRoster');
            teamRoster.innerHTML = '';

            // Add filled slots for team members
            userTeam.forEach((pokemon, index) => {
                const slot = document.createElement('div');
                slot.className = 'team-slot filled';
                slot.innerHTML = `
                    <div class="team-slot-content">
                        <div class="team-slot-sprite">${pokemon.image ? `<img src="${pokemon.image}" alt="${pokemon.name}" style="max-width: 50px;">` : '❓'}</div>
                        <div class="team-slot-name">${pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1)}</div>
                        <div class="team-slot-types">
                            ${pokemon.types.map(type => `<span class="team-slot-type-tag ${type}">${type}</span>`).join('')}
                        </div>
                        <button class="team-slot-remove" data-index="${index}" title="Remove from team">×</button>
                    </div>
                `;
                
                // Add remove handler
                const removeBtn = slot.querySelector('.team-slot-remove');
                removeBtn.addEventListener('click', (e) => {
                    e.stopPropagation();
                    removeFromTeam(index);
                });
                
                teamRoster.appendChild(slot);
            });

            // Add empty slots
            for (let i = userTeam.length; i < 6; i++) {
                const slot = document.createElement('div');
                slot.className = 'team-slot';
                slot.innerHTML = `<div class="team-slot-empty">Empty Slot</div>`;
                teamRoster.appendChild(slot);
            }

            // Update stats
            const stats = calculateTeamStats();
            const count = userTeam.length || 1;
            
            document.getElementById('statTotalHP').textContent = stats.totalHP;
            document.getElementById('statAvgHP').textContent = Math.round(stats.totalHP / count);
            document.getElementById('statTotalATK').textContent = stats.totalATK;
            document.getElementById('statAvgATK').textContent = Math.round(stats.totalATK / count);
            document.getElementById('statTotalDEF').textContent = stats.totalDEF;
            document.getElementById('statAvgDEF').textContent = Math.round(stats.totalDEF / count);
            document.getElementById('statTotalSPA').textContent = stats.totalSPA;
            document.getElementById('statAvgSPA').textContent = Math.round(stats.totalSPA / count);
            document.getElementById('statTotalSPD').textContent = stats.totalSPD;
            document.getElementById('statAvgSPD').textContent = Math.round(stats.totalSPD / count);
            document.getElementById('statTotalSpeed').textContent = stats.totalSpeed;
            document.getElementById('statAvgSpeed').textContent = Math.round(stats.totalSpeed / count);

            // Update type coverage
            const coverage = calculateTypeCoverage();
            const strongAgainstDiv = document.getElementById('strongAgainst');
            const weakToDiv = document.getElementById('weakTo');

            if (userTeam.length === 0) {
                strongAgainstDiv.innerHTML = '<span class="empty-coverage">No team selected</span>';
                weakToDiv.innerHTML = '<span class="empty-coverage">No team selected</span>';
            } else {
                strongAgainstDiv.innerHTML = coverage.strongAgainst.length > 0
                    ? coverage.strongAgainst.map(type => `<span class="coverage-type-tag">${type}</span>`).join('')
                    : '<span class="empty-coverage">None</span>';
                
                weakToDiv.innerHTML = coverage.weakTo.length > 0
                    ? coverage.weakTo.map(type => `<span class="coverage-type-tag">${type}</span>`).join('')
                    : '<span class="empty-coverage">None</span>';
            }

            // Update favorites selection
            renderFavoritesForTeam();
        }

        /**
         * Render favorites list for team building
         */
        function renderFavoritesForTeam() {
            const selectionDiv = document.getElementById('favoritesSelection');

            if (favorites.length === 0) {
                selectionDiv.innerHTML = '<div class="no-favorites">No favorite Pokémon yet. Add some from the Home page!</div>';
                return;
            }

            selectionDiv.innerHTML = favorites
                .map(pokemon => {
                    const inTeam = userTeam.some(p => p.name.toLowerCase() === pokemon.name.toLowerCase());
                    return `
                        <div class="favorite-for-team ${inTeam ? 'in-team' : ''}" data-pokemon-name="${pokemon.name}">
                            <div class="favorite-for-team-sprite">${pokemon.image ? `<img src="${pokemon.image}" alt="${pokemon.name}" style="max-width: 50px;">` : '❓'}</div>
                            <div class="favorite-for-team-name">${pokemon.name}</div>
                            ${inTeam ? '<div class="favorite-for-team-added">✓ In Team</div>' : ''}
                        </div>
                    `;
                })
                .join('');

            // Add click handlers
            selectionDiv.querySelectorAll('.favorite-for-team').forEach(element => {
                element.addEventListener('click', () => {
                    const pokemonName = element.getAttribute('data-pokemon-name');
                    const inTeam = userTeam.some(p => p.name.toLowerCase() === pokemonName.toLowerCase());
                    
                    if (inTeam) {
                        const index = userTeam.findIndex(p => p.name.toLowerCase() === pokemonName.toLowerCase());
                        removeFromTeam(index);
                    } else {
                        addToTeam(pokemonName);
                    }
                });
            });
        }

        /**
         * Initialize Team Builder page
         */
        function initializeTeamBuilder() {
            if (!currentUser) return;
            
            loadUserTeam();
            updateTeamDisplay();
        }

        // Tab switching (only if auth tabs exist)
        if (authTabBtns && authTabBtns.length > 0) {
            authTabBtns.forEach(btn => {
                btn.addEventListener('click', (e) => {
                    const tabName = btn.getAttribute('data-tab');
                    
                    // Update active tab button
                    authTabBtns.forEach(b => b.classList.remove('active'));
                    btn.classList.add('active');

                    // Update visible form
                    document.querySelectorAll('.auth-form').forEach(form => {
                        form.classList.remove('active');
                        form.style.display = 'none';
                    });
                    
                    const form = document.getElementById(`${tabName}Form`);
                    if (form) {
                        form.classList.add('active');
                        form.style.display = 'block';
                    }

                    // Clear error messages
                    const loginError = document.getElementById('loginError');
                    const signupError = document.getElementById('signupError');
                    if (loginError) loginError.textContent = '';
                    if (signupError) signupError.textContent = '';
                });
            });
        }

        // AUTHENTICATION REMOVED - No login/signup/logout needed

        // ============================================================
        // PROFILE PAGE EVENT HANDLERS
        // ============================================================

        const saveProfileBtn = document.getElementById('saveProfileBtn');
        const displayNameInput = document.getElementById('displayName');

        // Save profile on button click
        if (saveProfileBtn) {
            saveProfileBtn.addEventListener('click', (e) => {
                e.preventDefault();
                saveProfile();
            });
        }

        // Update preview as user types
        if (displayNameInput) {
            displayNameInput.addEventListener('input', (e) => {
                const displayName = e.target.value;
                updateProfilePreview(displayName, selectedAvatar || AVATAR_OPTIONS[0]);
            });
        }

        // ROUTING FUNCTIONS
        function navigateTo(viewName) {
            // Hide all views
            views.forEach(view => view.classList.remove('active'));
            
            // Remove active class from all nav links
            navLinks.forEach(link => link.classList.remove('active'));
            
            // Show the selected view
            const selectedView = document.getElementById(viewName);
            if (selectedView) {
                selectedView.classList.add('active');
            }
            
            // Highlight the active nav link
            const activeLink = document.querySelector(`[data-view="${viewName}"]`);
            if (activeLink) {
                activeLink.classList.add('active');
            }
            
            currentView = viewName;
            
            // Load view-specific data
            if (viewName === 'favorites') {
                renderFavoritesView();
            } else if (viewName === 'profile') {
                initializeProfilePage();
            } else if (viewName === 'team') {
                initializeTeamBuilder();
            }
        }

        /**
         * Render favorites in the favorites view (separate from home view favorites)
         */
        function renderFavoritesView() {
            if (favorites.length === 0) {
                favoritesViewGrid.innerHTML = '<div class="no-favorites">No favorite Pokémon yet</div>';
                return;
            }

            favoritesViewGrid.innerHTML = favorites
                .map((pokemon) => `
                    <div class="favorite-card" title="${pokemon.name}">
                        ${pokemon.image ? `<img src="${pokemon.image}" alt="${pokemon.name}">` : '<div style="height: 80px; display: flex; align-items: center; justify-content: center;">❓</div>'}
                        <div class="favorite-card-name">${pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1)}</div>
                        <button class="favorite-card-remove" aria-label="Remove ${pokemon.name} from favorites" data-name="${pokemon.name}">✕</button>
                    </div>
                `)
                .join('');

            // Add event listeners to remove buttons
            favoritesViewGrid.querySelectorAll('.favorite-card-remove').forEach((btn) => {
                btn.addEventListener('click', (e) => {
                    e.stopPropagation();
                    removeFavorite(btn.dataset.name);
                });
            });

            // Add click handlers to favorite cards for quick search
            favoritesViewGrid.querySelectorAll('.favorite-card').forEach((card) => {
                card.addEventListener('click', () => {
                    if (!isCompareMode) {
                        navigateTo('home');
                        searchInput.value = card.querySelector('.favorite-card-name').textContent.toLowerCase();
                        searchPokemonByName(card.querySelector('.favorite-card-name').textContent.toLowerCase());
                    }
                });
            });
        }

        // Add nav link click handlers
        navLinks.forEach(link => {
            link.addEventListener('click', (e) => {
                e.preventDefault();
                const viewName = link.getAttribute('data-view');
                navigateTo(viewName);
            });
        });

        // Handle form submission
        searchForm.addEventListener('submit', (e) => {
            e.preventDefault();
            searchPokemon();
        });

        // Handle Enter key in input
        searchInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                e.preventDefault();
                searchPokemon();
            }
        });

        // Handle random button
        randomBtn.addEventListener('click', () => {
            const randomId = Math.floor(Math.random() * MAX_POKEMON_ID) + 1;
            searchPokemonById(randomId);
        });

        // Update sound toggle display
        updateSoundToggle();

        soundToggle.addEventListener('click', () => {
            isSoundEnabled = !isSoundEnabled;
            localStorage.setItem('soundEnabled', JSON.stringify(isSoundEnabled));
            updateSoundToggle();
        });

        function updateSoundToggle() {
            soundToggle.textContent = isSoundEnabled ? '🔊' : '🔇';
            soundToggle.classList.toggle('muted', !isSoundEnabled);
        }

        compareModeBtn.addEventListener('click', () => {
            isCompareMode = !isCompareMode;
            compareModeBtn.classList.toggle('active', isCompareMode);
            searchForm.style.display = isCompareMode ? 'none' : 'block';
            compareForm.style.display = isCompareMode ? 'block' : 'none';
            resultsSection.innerHTML = '<div class="empty-state">Search for Pokémon to compare.</div>';
            comparePokemon = [null, null];
            document.body.className = '';
        });

        compareSearch1.addEventListener('click', (e) => {
            e.preventDefault();
            compareSearchPokemon(0, compareInput1.value.trim());
        });

        compareSearch2.addEventListener('click', (e) => {
            e.preventDefault();
            compareSearchPokemon(1, compareInput2.value.trim());
        });

        compareInput1.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                e.preventDefault();
                compareSearchPokemon(0, compareInput1.value.trim());
            }
        });

        compareInput2.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                e.preventDefault();
                compareSearchPokemon(1, compareInput2.value.trim());
            }
        });

        function updateSearchHistory(pokemonName) {
            // Remove if already exists, then add to front
            searchHistory = searchHistory.filter(p => p !== pokemonName);
            searchHistory.unshift(pokemonName);
            // Keep only last 5
            searchHistory = searchHistory.slice(0, MAX_HISTORY);
            localStorage.setItem('pokemonHistory', JSON.stringify(searchHistory));
            renderHistoryChips();
        }

        function renderHistoryChips() {
            if (searchHistory.length === 0) {
                historyChipsContainer.innerHTML =
                    '<div class="empty-history">No recent searches yet</div>';
                return;
            }
            historyChipsContainer.innerHTML = searchHistory
                .map(
                    (pokemon) =>
                        `<button type="button" class="history-chip" data-pokemon="${pokemon}">${pokemon}</button>`
                )
                .join('');
            // Add click handlers
            historyChipsContainer.querySelectorAll('.history-chip').forEach((chip) => {
                chip.addEventListener('click', (e) => {
                    e.preventDefault();
                    const pokemon = chip.dataset.pokemon;
                    searchInput.value = pokemon;
                    searchPokemonByName(pokemon);
                });
            });
        }

        async function searchPokemon() {
            const query = searchInput.value.trim().toLowerCase();

            if (!query) {
                // Shake the input field
                searchInput.classList.remove('shake');
                void searchInput.offsetWidth; // Trigger reflow to restart animation
                searchInput.classList.add('shake');
                resultsSection.innerHTML =
                    '<div class="empty-state">Please enter a Pokémon name to search.</div>';
                return;
            }

            searchPokemonByName(query);
        }

        function playSound() {
            if (!isSoundEnabled) return;
            
            // Simple Web Audio API beep
            try {
                const audioContext = new (window.AudioContext || window.webkitAudioContext)();
                const oscillator = audioContext.createOscillator();
                const gainNode = audioContext.createGain();
                
                oscillator.connect(gainNode);
                gainNode.connect(audioContext.destination);
                
                oscillator.frequency.value = 600;
                oscillator.type = 'sine';
                
                gainNode.gain.setValueAtTime(0.3, audioContext.currentTime);
                gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.2);
                
                oscillator.start(audioContext.currentTime);
                oscillator.stop(audioContext.currentTime + 0.2);
            } catch (e) {
                // Audio context not supported
            }
        }

        function updateBackgroundColor(pokemon) {
            const primaryType = pokemon.types[0].type.name;
            document.body.className = `type-${primaryType}`;
        }

        async function compareSearchPokemon(index, query) {
            if (!query) {
                alert('Please enter a Pokémon name!');
                return;
            }

            try {
                const response = await fetch(`${API_BASE_URL}/${query.toLowerCase()}`);
                if (!response.ok) throw new Error('Not found');
                
                const pokemon = await response.json();
                comparePokemon[index] = pokemon;
                
                if (comparePokemon[0] && comparePokemon[1]) {
                    displayComparison();
                }
            } catch (error) {
                alert(`Pokémon "${query}" not found!`);
            }
        }

        function displayComparison() {
            const [p1, p2] = comparePokemon;
            
            const statMap1 = {};
            p1.stats.forEach((stat) => {
                statMap1[stat.stat.name] = stat.base_stat;
            });
            
            const statMap2 = {};
            p2.stats.forEach((stat) => {
                statMap2[stat.stat.name] = stat.base_stat;
            });
            
            const createCard = (pokemon, statMap) => `
                <div class="compare-card">
                    <h3 class="pokemon-name" style="text-align: center; margin-bottom: 15px;">${pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1)}</h3>
                    <div style="text-align: center; margin-bottom: 15px;">
                        ${pokemon.sprites.front_default ? `<img src="${pokemon.sprites.front_default}" alt="${pokemon.name}" style="max-width: 100px;">` : '❓'}
                    </div>
                    <div class="pokemon-stats" style="font-size: 0.9rem;">
                        <div class="stat-row"><span class="stat-label">HP</span><span class="stat-value">${statMap['hp'] || '—'}</span></div>
                        <div class="stat-row"><span class="stat-label">Attack</span><span class="stat-value">${statMap['attack'] || '—'}</span></div>
                        <div class="stat-row"><span class="stat-label">Defense</span><span class="stat-value">${statMap['defense'] || '—'}</span></div>
                        <div class="stat-row"><span class="stat-label">Sp. Atk</span><span class="stat-value">${statMap['spa'] || '—'}</span></div>
                        <div class="stat-row"><span class="stat-label">Sp. Def</span><span class="stat-value">${statMap['spd'] || '—'}</span></div>
                        <div class="stat-row"><span class="stat-label">Speed</span><span class="stat-value">${statMap['speed'] || '—'}</span></div>
                    </div>
                </div>
            `;
            
            resultsSection.innerHTML = `<div class="compare-results">${createCard(p1, statMap1)}${createCard(p2, statMap2)}</div>`;
            playSound();
        }

        async function searchPokemonByName(query) {
            query = query.toLowerCase();

            // Show loading spinner
            resultsSection.innerHTML =
                '<div class="loading-spinner"><div class="spinner"></div></div>';

            try {
                const response = await fetch(`${API_BASE_URL}/${query}`);

                if (!response.ok) {
                    throw new Error('Pokémon not found');
                }

                const pokemon = await response.json();
                updateSearchHistory(pokemon.name);
                updateBackgroundColor(pokemon);
                displayPokemon(pokemon);
                playSound();
            } catch (error) {
                resultsSection.innerHTML =
                    `<div class="empty-state">❌ Pokémon "<strong>${query}</strong>" not found.<br><br>Please check the spelling and try again!</div>`;
            }
        }

        async function searchPokemonById(id) {
            // Show loading spinner
            resultsSection.innerHTML =
                '<div class="loading-spinner"><div class="spinner"></div></div>';

            try {
                const response = await fetch(`${API_BASE_URL}/${id}`);

                if (!response.ok) {
                    throw new Error('Pokémon not found');
                }

                const pokemon = await response.json();
                searchInput.value = pokemon.name;
                updateSearchHistory(pokemon.name);
                updateBackgroundColor(pokemon);
                displayPokemon(pokemon);
                playSound();
            } catch (error) {
                resultsSection.innerHTML =
                    '<div class="empty-state">❌ Pokémon not found. Please try again!</div>';
            }
        }

        function displayPokemon(pokemon) {
            // Extract types from API response
            const types = pokemon.types.map((t) => t.type.name);
            const typesHTML = types
                .map(
                    (type) =>
                        `<span class="type-tag ${type}">${type}</span>`
                )
                .join('');

            // Extract stats from API response
            const statMap = {};
            pokemon.stats.forEach((stat) => {
                statMap[stat.stat.name] = stat.base_stat;
            });

            // Get sprite image
            const imageUrl = pokemon.sprites.front_default;
            const imageHTML = imageUrl
                ? `<img src="${imageUrl}" alt="${pokemon.name}" style="max-width: 120px; image-rendering: pixelated;">`
                : '<div style="font-size: 80px;">❓</div>';

            const html = `
                <div class="pokemon-card" style="position: relative;">
                    <button class="favorite-btn ${isFavorited(pokemon.name) ? 'favorited' : ''}" data-pokemon-name="${pokemon.name}" title="Add to favorites">
                        ${isFavorited(pokemon.name) ? '❤️' : '🤍'}
                    </button>
                    <div class="pokemon-header">
                        <h2 class="pokemon-name">${pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1)}</h2>
                        <span class="pokemon-id">#${String(pokemon.id).padStart(3, '0')}</span>
                    </div>

                    <div class="pokemon-image">
                        ${imageHTML}
                    </div>

                    <div class="pokemon-types">${typesHTML}</div>

                    <div class="pokemon-dimensions">
                        <div class="dimension">
                            <div class="dimension-label">Height</div>
                            <div class="dimension-value">${(pokemon.height / 10).toFixed(1)} m</div>
                        </div>
                        <div class="dimension">
                            <div class="dimension-label">Weight</div>
                            <div class="dimension-value">${(pokemon.weight / 10).toFixed(1)} kg</div>
                        </div>
                    </div>

                    <div class="pokemon-stats">
                        <div class="stat-row">
                            <span class="stat-label">HP</span>
                            <span class="stat-value">${statMap['hp'] || '—'}</span>
                        </div>
                        <div class="stat-row">
                            <span class="stat-label">Attack</span>
                            <span class="stat-value">${statMap['attack'] || '—'}</span>
                        </div>
                        <div class="stat-row">
                            <span class="stat-label">Defense</span>
                            <span class="stat-value">${statMap['defense'] || '—'}</span>
                        </div>
                        <div class="stat-row">
                            <span class="stat-label">Sp. Atk</span>
                            <span class="stat-value">${statMap['spa'] || '—'}</span>
                        </div>
                        <div class="stat-row">
                            <span class="stat-label">Sp. Def</span>
                            <span class="stat-value">${statMap['spd'] || '—'}</span>
                        </div>
                        <div class="stat-row">
                            <span class="stat-label">Speed</span>
                            <span class="stat-value">${statMap['speed'] || '—'}</span>
                        </div>
                    </div>
                </div>
            `;

            resultsSection.innerHTML = html;

            // Add favorite button event listener
            const favoriteBtn = resultsSection.querySelector('.favorite-btn');
            if (favoriteBtn) {
                favoriteBtn.addEventListener('click', (e) => {
                    e.preventDefault();
                    if (isFavorited(pokemon.name)) {
                        removeFavorite(pokemon.name);
                    } else {
                        addFavorite(pokemon);
                    }
                    // Update button appearance
                    favoriteBtn.classList.toggle('favorited');
                    favoriteBtn.textContent = isFavorited(pokemon.name) ? '❤️' : '🤍';
                });
            }
        }

        // Initialize IndexedDB
        initializeDB().then(() => {
            initializeAuth();
            if (currentUser) {
                loadUserFavorites();
            }
            renderHistoryChips();
        });

        function initializeDB() {
            return new Promise((resolve, reject) => {
                const request = indexedDB.open('pokedex_db', 1);

                request.onerror = () => {
                    console.error('IndexedDB failed to open');
                    reject(request.error);
                };

                request.onsuccess = () => {
                    db = request.result;
                    dbReady = true;
                    console.log('IndexedDB initialized successfully');
                    resolve();
                };

                request.onupgradeneeded = (event) => {
                    const database = event.target.result;
                    if (!database.objectStoreNames.contains('favorites')) {
                        database.createObjectStore('favorites', { keyPath: 'id', autoIncrement: true });
                        console.log('Created favorites object store');
                    }
                };
            });
        }

        /**
         * Load favorites for the current logged-in user
         */
        function loadUserFavorites() {
            if (!dbReady || !currentUser) return;
            
            const transaction = db.transaction('favorites', 'readonly');
            const objectStore = transaction.objectStore('favorites');
            const request = objectStore.getAll();

            request.onsuccess = () => {
                const allFavorites = request.result;
                // Filter favorites for current user
                favorites = allFavorites.filter(fav => fav.username === currentUser);
                updateFavoritesCounter();
            };
        }

        function loadFavoritesFromDB() {
            // This is called on page load via initializeDB
            // Instead we use loadUserFavorites which filters by current user
            loadUserFavorites();
        }

        function saveFavoriteToDB(pokemon) {
            if (!dbReady || !currentUser) return;
            
            const transaction = db.transaction('favorites', 'readwrite');
            const objectStore = transaction.objectStore('favorites');
            
            const favoriteData = {
                username: currentUser,
                name: pokemon.name,
                image: pokemon.image,
                types: pokemon.types,
                timestamp: new Date().toISOString()
            };
            
            const request = objectStore.add(favoriteData);

            request.onerror = () => {
                console.error('Failed to save favorite:', request.error);
            };
        }

        function removeFavoriteFromDB(pokemonName) {
            if (!dbReady || !currentUser) return;
            
            const transaction = db.transaction('favorites', 'readonly');
            const objectStore = transaction.objectStore('favorites');
            const request = objectStore.getAll();

            request.onsuccess = () => {
                const allFavorites = request.result;
                const toDelete = allFavorites.find(fav => 
                    fav.username === currentUser && fav.name.toLowerCase() === pokemonName.toLowerCase()
                );

                if (toDelete) {
                    const deleteTransaction = db.transaction('favorites', 'readwrite');
                    const deleteStore = deleteTransaction.objectStore('favorites');
                    deleteStore.delete(toDelete.id);
                }
            };
        }

        function updateFavoritesCounter() {
            favoritesCounter.textContent = `(${favorites.length})`;
        }

        function exportFavoritesAsJSON() {
            if (favorites.length === 0) {
                alert('No favorites to export!');
                return;
            }

            const dataToExport = favorites.map(fav => ({
                name: fav.name,
                image: fav.image,
                types: fav.types
            }));

            const json = JSON.stringify(dataToExport, null, 2);
            const blob = new Blob([json], { type: 'application/json' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `pokemon-favorites-${new Date().toISOString().split('T')[0]}.json`;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
        }

        exportBtn.addEventListener('click', exportFavoritesAsJSON);

        function isFavorited(pokemonName) {
            return favorites.some(fav => fav.name.toLowerCase() === pokemonName.toLowerCase());
        }

        function addFavorite(pokemon) {
            if (!isFavorited(pokemon.name)) {
                const favorite = {
                    name: pokemon.name,
                    image: pokemon.sprites.front_default,
                    types: pokemon.types.map(t => t.type.name),
                    stats: pokemon.stats
                };
                favorites.push(favorite);
                saveFavoriteToDB(favorite);
                renderFavorites();
                updateFavoritesCounter();
                playSound();
            }
        }

        function removeFavorite(pokemonName) {
            favorites = favorites.filter(fav => fav.name.toLowerCase() !== pokemonName.toLowerCase());
            removeFavoriteFromDB(pokemonName);
            renderFavorites();
            updateFavoritesCounter();
        }

        function renderFavorites() {
            if (favorites.length === 0) {
                favoritesSection.style.display = 'none';
                return;
            }

            favoritesSection.style.display = 'block';
            favoritesGrid.innerHTML = favorites
                .map((pokemon) => `
                    <div class="favorite-card" title="${pokemon.name}">
                        ${pokemon.image ? `<img src="${pokemon.image}" alt="${pokemon.name}">` : '<div style="height: 80px; display: flex; align-items: center; justify-content: center;">❓</div>'}
                        <div class="favorite-card-name">${pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1)}</div>
                        <button class="favorite-card-remove" aria-label="Remove ${pokemon.name} from favorites" data-name="${pokemon.name}">✕</button>
                    </div>
                `)
                .join('');

            // Add event listeners to remove buttons
            favoritesGrid.querySelectorAll('.favorite-card-remove').forEach((btn) => {
                btn.addEventListener('click', (e) => {
                    e.stopPropagation();
                    removeFavorite(btn.dataset.name);
                });
            });

            // Add click handlers to favorite cards for quick search
            favoritesGrid.querySelectorAll('.favorite-card').forEach((card) => {
                card.addEventListener('click', () => {
                    if (!isCompareMode) {
                        searchInput.value = card.querySelector('.favorite-card-name').textContent.toLowerCase();
                        searchPokemonByName(card.querySelector('.favorite-card-name').textContent.toLowerCase());
                    }
                });
            });
        }
    </script>
</body>
</html>
