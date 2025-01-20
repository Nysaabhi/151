<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Deep Chatbot</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/flatpickr/4.6.13/flatpickr.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600&display=swap" rel="stylesheet">
    <style>
:root { 
    --primary-color: #FFD700; 
    --secondary-color: #FDB931; 
    --background-dark: #1a1a1f; 
    --text-light: #ffffff; 
    --text-dark: #000000; 
    --accent-gradient: linear-gradient(135deg, var(--primary-color), var(--secondary-color)); 
    --error-color: #ff4444; 
    --success-color: #00C851; 
}

* { 
    box-sizing: border-box; 
    margin: 0; 
    padding: 0; 
}

body { 
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif; 
    background-color: var(--background-dark); 
    color: var(--text-light); 
    line-height: 1.6; 
    overflow-x: hidden; 
}

.header { 
    padding: 8px 16px; 
    background: rgba(26, 26, 31, 0.95); 
    position: fixed; 
    width: 100%; 
    top: 0; 
    left: 0; 
    z-index: 30; 
    border-bottom: 2px solid var(--primary-color); 
    backdrop-filter: blur(10px); 
    height: 60px; 
}

.header-content { 
    max-width: 1400px; 
    margin: 0 auto; 
    display: flex; 
    justify-content: space-between; 
    align-items: center; 
    height: 100%; 
}

.logo { 
    font-size: 1.8em; 
    font-weight: 900; 
    background: var(--accent-gradient); 
    -webkit-background-clip: text; 
    color: transparent; 
    letter-spacing: 1.2px; 
}

.menu-button { 
    padding: 10px 20px; 
    background: var(--accent-gradient); 
    border: none; 
    border-radius: 50px; 
    color: var(--text-dark); 
    font-weight: 600; 
    cursor: pointer; 
    transition: all 0.3s ease; 
    font-size: 0.95em; 
}

.chatbot-container { 
    position: fixed; 
    top: 60px; 
    left: 0; 
    right: 0; 
    bottom: 60px; 
    background: rgba(26, 26, 31, 0.95); 
    display: flex; 
    flex-direction: column; 
}

.chat-header { 
    padding: 15px; 
    background: linear-gradient(135deg, rgba(255, 215, 0, 0.1), rgba(253, 185, 49, 0.1)); 
    border-bottom: 1px solid rgba(255, 215, 0, 0.2); 
    display: flex; 
    justify-content: space-between; 
    align-items: center; 
}

.chat-status { 
    display: flex; 
    align-items: center; 
    color: var(--primary-color); 
    font-weight: 600; 
}

.status-dot { 
    width: 8px; 
    height: 8px; 
    background: var(--primary-color); 
    border-radius: 50%; 
    margin-right: 8px; 
    animation: pulse 2s infinite; 
}

.chat-messages { 
    flex: 1; 
    overflow-y: auto; 
    padding: 20px; 
    scroll-behavior: smooth; 
}

/* Keep existing styles */
.message {
    display: flex;
    gap: 10px;
    max-width: 85%;
    margin-bottom: 16px;
    animation: messageSlide 0.3s ease-out;
}

/* Updated bot message styles to match ChatGPT */
.bot-message {
    align-self: flex-start;
    max-width: 95%;
    width: 100%;
}

.bot-message .message-content {
    background: transparent;
    padding: 16px 48px 16px 16px;
    border-radius: 0;
    color: var(--text-light);
    font-size: 16px;
    line-height: 1.6;
    position: relative;
}

/* Keep user message styles as they are */
.user-message {
    align-self: flex-end;
    flex-direction: row-reverse;
}

.user-message .message-content {
    background: rgba(255, 215, 0, 0.1);
    border-radius: 16px;
}

.welcome-content {
   background: rgba(255, 255, 255, 0.05); 
   padding: 12px 16px;
    border-radius: 16px;
    color: var(--text-light);
}

.welcome-avatar { 
     width: 36px;
    height: 36px;
    min-width: 36px;
    min-height: 36px;
    background: rgba(255, 215, 0, 0.1);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--primary-color);
    flex-shrink: 0;
}

/* Animation for welcome message */
.welcome-message {
    animation: messageSlide 0.3s ease-out;
}

/* Responsive adjustments */
@media (max-width: 768px) {
    .welcome-message {
        max-width: 90%;
    }
}

/* Fade out animation */
.welcome-message.fade-out {
    opacity: 0;
    transition: opacity 0.3s ease-out;
}

/* Add markdown styling for bot messages */
.bot-message .message-content pre {
    background: rgba(255, 255, 255, 0.05);
    border-radius: 6px;
    padding: 12px;
    margin: 8px 0;
    overflow-x: auto;
}

.bot-message .message-content code {
    background: rgba(255, 255, 255, 0.1);
    padding: 2px 6px;
    border-radius: 4px;
    font-family: 'Menlo', 'Monaco', 'Courier New', monospace;
}

.bot-message .message-content p {
    margin-bottom: 16px;
}

.bot-message .message-content ul,
.bot-message .message-content ol {
    margin: 16px 0;
    padding-left: 24px;
}

.bot-message .message-content li {
    margin-bottom: 8px;
}

/* Add a subtle separator between messages */
.bot-message:not(:last-child) {
    border-bottom: 1px solid rgba(255, 255, 255, 0.05);
    padding-bottom: 16px;
}

/* Responsive adjustments */
@media (max-width: 768px) {
    .bot-message {
        max-width: 100%;
    }
    
    .bot-message .message-content {
        padding: 12px 24px 12px 12px;
    }
}

.message-avatar { 
    width: 36px; 
    height: 36px; 
    min-width: 36px; 
    min-height: 36px; 
    background: rgba(255, 215, 0, 0.1); 
    border-radius: 50%; 
    display: flex; 
    align-items: center; 
    justify-content: center; 
    color: var(--primary-color); 
    flex-shrink: 0; 
}

.message-content { 
    background: rgba(255, 255, 255, 0.05); 
    padding: 12px 16px; 
    border-radius: 16px; 
    color: var(--text-light); 
}

.user-message .message-content { 
    background: rgba(255, 215, 0, 0.1); 
}

.chat-input-container {
  padding: 20px;
  background: rgba(26, 26, 31, 0.98);
  border-top: 1px solid rgba(255, 215, 0, 0.1);
  display: flex;
  gap: 12px;
  align-items: center;
}

.input-with-voice {
  flex: 1;
  position: relative;
  display: flex;
  align-items: center;
}

#chatInput {
  width: 100%;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.2);
  border-radius: 12px;
  padding: 20px 50px 20px 20px;
  color: var(--text-light);
  font-size: 0.95em;
}

.voice-button {
  position: absolute;
  right: 10px;
  background: none;
  border: none;
  color: var(--primary-color);
  cursor: pointer;
  padding: 10px;
  transition: all 0.3s ease;
}

.voice-button:hover {
  color: var(--secondary-color);
}

.send-message {
  padding: 20px;
  background: var(--accent-gradient);
  border: none;
  border-radius: 8px;
  color: #ffffff;
  cursor: pointer;
  transition: all 0.3s ease;
}

/* Desktop styles */
@media (min-width: 769px) {
  .chat-input-container {
    width: 60%;
    margin-left: auto;
    margin-right: auto;
  }
  
  #chatInput {
    width: 100%;
    height: 80px;
    font-size: 1.05rem;
    padding: 0 70px 0 24px;  /* Increased right padding for larger voice button */
    border-radius: 16px;
  }
  
  .voice-button {
    width: 60px;  /* Increased width */
    height: 60px; /* Increased height */
    right: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .voice-button i {
    font-size: 2.1em;  /* Increased font size */
  }
  
  .send-message {
    padding: 20px;
    height: 80px;
    width: 80px;  /* Made slightly wider */
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .send-message i {
    font-size: 2.1em;  /* Increased font size */
  }
}

/* Mobile styles */
@media (max-width: 768px) {
  .chat-input-container {
    width: 100%;
    padding: 10px;
  }
  
  #chatInput {
    width: 100%;
  }
  
  .voice-button i {
    font-size: 1.5em;
  }
  
  .send-message i {
    font-size: 1.5em;
  }
}

.alert { 
    position: fixed; 
    top: 20px; 
    right: 20px; 
    padding: 15px 20px; 
    border-radius: 8px; 
    color: var(--text-light); 
    z-index: 1000; 
    animation: slideIn 0.3s ease-out; 
}

.alert-success { 
    background: var(--success-color); 
}

.alert-error { 
    background: var(--error-color); 
}

.bottom-nav { 
    position: fixed; 
    bottom: 0; 
    left: 0; 
    right: 0; 
    height: 60px; 
    background: rgba(26, 26, 31, 0.98); 
    padding: 8px 0; 
    border-top: 1px solid rgba(255, 215, 0, 0.2); 
}

.nav-container { 
    display: flex; 
    justify-content: space-around; 
    align-items: center; 
    height: 100%; 
    max-width: 600px; 
    margin: 0 auto; 
}

.nav-item { 
    display: flex; 
    flex-direction: column; 
    align-items: center; 
    text-decoration: none; 
    color: #fff; 
    transition: all 0.3s ease; 
    padding: 5px; 
    cursor: pointer; 
    font-family: 'Poppins', sans-serif; 
}

.nav-item i { 
    color: #ffffff; 
    font-size: 20px; 
    margin-bottom: 4px; 
}

.nav-item span { 
    font-size: 12px; 
    font-family: 'Poppins', sans-serif; 
    font-weight: 600; 
    letter-spacing: 0.6px; 
    text-align: center; 
}

@keyframes pulse { 
    0% { opacity: 1; } 
    50% { opacity: 0.5; } 
    100% { opacity: 1; } 
}

@keyframes messageSlide { 
    from { opacity: 0; transform: translateY(10px); } 
    to { opacity: 1; transform: translateY(0); } 
}

@keyframes slideIn { 
    from { opacity: 0; transform: translateX(100px); } 
    to { opacity: 1; transform: translateX(0); } 
}

@media (max-width: 768px) { 
    .message { 
        max-width: 90%; 
    }
}

body > h1:first-of-type:not(.heading) { 
    display: none !important; 
}

.markdown-body h1:first-child { 
    display: none !important; 
}

.position-relative h1:first-child { 
    display: none !important; 
}
      
      .modal-overlay { 
          position: absolute; 
          top: 0; 
          left: 0; 
          width: 100%; 
          height: 100%; 
          background-color: rgba(0, 0, 0, 0.5); 
          display: flex; 
          justify-content: center; 
          align-items: center; 
          padding: 20px; 
      }
      
      .modal-content { 
          background-color: #1a1a1f; 
          border-radius: 12px; 
          width: 100%; 
          max-width: 600px; 
          max-height: 90vh; 
          overflow-y: auto; 
          box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15); 
      }
      
      .modal-header { 
          padding: 20px; 
          border-bottom: 1px solid #eee; 
          display: flex; 
          justify-content: space-between; 
          align-items: center; 
      }
      
      .modal-header h2 { 
          margin: 0; 
          font-size: 1.5rem; 
          color: #fffdfd; 
      }
      
      .close-button { 
          background: none; 
          border: none; 
          font-size: 1.5rem; 
          cursor: pointer; 
          color: #b1b1b1; 
          padding: 5px; 
      }
      
      .modal-body { 
          padding: 20px; 
      }
      
      .menu-overlay { 
          display: none; 
          position: fixed; 
          top: 60px; 
          left: 0; 
          right: 0; 
          bottom: 60px; 
          background: rgba(26, 26, 31, 0.95); 
          z-index: 25; 
          padding: 0; 
          animation: fadeIn 0.3s ease-out; 
          overflow-y: auto; 
      }
      
      .menu-sections { 
          background: rgba(26, 26, 31, 0.95); 
          padding: 20px; 
      }
      
      .menu-section { 
        background: rgba(26, 26, 31, 0.95); 
          margin-bottom: 24px; 
      }
      
      .menu-section-title { 
          color: var(--primary-color); 
          font-size: 1.2em; 
          font-weight: 600; 
          margin-bottom: 12px; 
          padding-bottom: 8px; 
          border-bottom: 1px solid rgba(255, 215, 0, 0.2); 
      }
      
      .menu-items { 
          display: flex; 
          flex-direction: column; 
          gap: 8px; 
      }
      
      .menu-item { 
          padding: 12px 16px; 
          background: rgba(255, 255, 255, 0.05); 
          border-radius: 8px; 
          color: var(--text-light); 
          cursor: pointer; 
          transition: all 0.3s ease; 
          display: flex; 
          justify-content: space-between; 
          align-items: center; 
      }
      
      .menu-item:hover { 
          background: rgba(255, 215, 0, 0.1); 
      }
      
      .menu-item-price { 
          color: var(--primary-color); 
          font-weight: 600; 
      }
      
      .social-links { 
          display: flex; 
          gap: 12px; 
      }
      
      .social-link { 
          width: 40px; 
          height: 40px; 
          border-radius: 50%; 
          background: rgba(255, 255, 255, 0.05); 
          display: flex; 
          align-items: center; 
          justify-content: center; 
          color: var(--text-light); 
          text-decoration: none; 
          transition: all 0.3s ease; 
      }
      
      .social-link:hover { 
          background: var(--primary-color); 
          color: var(--text-dark); 
      }
      
      .about-content { 
          line-height: 1.6; 
          color: #cccccc; 
      }
      
      .menu-item { 
          animation: slideIn 0.3s ease-out; 
          animation-fill-mode: both; 
      }
      
      .menu-item:nth-child(1) { 
          animation-delay: 0.1s; 
      }
      
      .menu-item:nth-child(2) { 
          animation-delay: 0.2s; 
      }
      
      .menu-item:nth-child(3) { 
          animation-delay: 0.3s; 
      }
      
      .menu-item:nth-child(4) { 
          animation-delay: 0.4s; 
      }
      
      .menu-item:nth-child(5) { 
          animation-delay: 0.5s; 
      }
      
      @keyframes slideIn { 
          from { 
              opacity: 0; 
              transform: translateX(-20px); 
          } 
          to { 
              opacity: 1; 
              transform: translateX(0); 
          } 
      }
      
      .deepchat-item { 
          background-color: #ff0000; 
          color: rgb(255, 255, 255); 
          border-radius: 8px; 
          padding: 10px; 
          text-align: center; 
      }
      
      .deepchat-item i { 
          color: rgb(255, 255, 5); 
      }
      
      .deepchat-item:hover { 
          background-color: #ff0606; 
          color: rgb(255, 255, 255); 
      }
      
      .deepchat-item span { 
          font-weight: 600; 
          font-family: 'Poppins', -apple-system, BlinkMacSystemFont, Roboto, sans-serif; 
          letter-spacing: 0.3px; 
      }
      
      .confirmation-icon { 
          font-size: 80px; 
          color: #4CAF50; 
          margin-bottom: 20px; 
      }
      
      .confirmation-icon i { 
          display: block; 
      }
      
      .alert { 
          position: fixed; 
          top: 20px; 
          left: 50%; 
          transform: translateX(-50%); 
          padding: 10px 20px; 
          border-radius: 5px; 
          z-index: 1100; 
      }
      
      .alert-success { 
          background-color: #4CAF50; 
          color: white; 
      }
      
      .alert-error { 
          background-color: #f44336; 
          color: white; 
      }
      
      .location-links, .partner-links { 
          display: flex; 
          flex-direction: column; 
      } 
      
      .location-link, .partner-link { 
          text-decoration: none; 
          color: #ffffff; 
          padding: 8px 10px; 
          transition: all 0.3s ease; 
          border-radius: 4px; 
      } 
      
      .location-link:hover, .partner-link:hover { 
          background-color: #f0f0f0; 
          color: #333; 
      } 
      
      .location-link i, .partner-link i { 
          margin-right: 10px; 
          color: #fff; 
      } 
      
      .location-link:hover i, .partner-link:hover i { 
          color: #2c7cd1; 
      }
      
      .header-button-group { 
          display: flex; 
          align-items: center; 
          gap: 5px; 
      }
      
      .town-button, 
      .catalogue-button, 
      .deals-button { 
          display: flex; 
          align-items: center; 
          justify-content: center; 
          padding: 15px; 
          background: none; 
          border: none; 
          color: var(--primary-color); 
          cursor: pointer; 
          transition: background-color 0.3s ease; 
          border-radius: 4px; 
          width: 40px; 
          height: 40px; 
      }
      
      .town-button:hover, 
      .catalogue-button:hover, 
      .deals-button:hover { 
          background: rgba(255, 215, 0, 0.1); 
      }
      
      .town-button i, 
      .catalogue-button i, 
      .deals-button i { 
          font-size: 18px; 
      }
      
      </style>        
        </head>
        <body>
            <header class="header">
                <div class="header-content">
                    <a href="https://nysaabhi.github.io/A2" class="logo">Deep</a>
                    <button class="menu-button" id="menuButton">
                        <i class="fas fa-bars"></i>
                     Menu
                    </button>
            </div>
            </header>
            
            <div class="chatbot-container">
                <div class="chat-header">
                    <div class="chat-status">
                        <span class="status-dot"></span>
                        Deep AI Assistant
                    </div>
                    <div class="header-button-group">
                        <button class="town-button" id="townButton">
                            <i class="fas fa-map-marker-alt"></i>
                        </button>
                        <button class="catalogue-button" id="catalogueButton">
                            <i class="fas fa-image"></i>
                        </button>
                        <button class="deals-button" id="dealsButton">
                            <i class="fas fa-gift"></i>
                        </button>
                    </div>
                </div>
        
                <div class="chat-messages" id="chatMessages">
                    <div class="category-grid" id="categoryGrid">
                        <!-- Categories will be dynamically populated -->
                    </div>
                             
                    <div class="woohoo" id="woohoo">
                        <div class="hahaha">
                    </div>
                </div>
            </div>

            
            <!-- Bottom Navigation -->
            <nav class="bottom-nav">
                <div class="nav-container">
                    <div class="nav-item" data-page="services">
                           <i class="fas fa-user-tie"></i>
                        <span>Service</span>
                    </div>
                    <div class="nav-item" data-page="brand">
                        <i class="fas fa-store"></i>
                        <span>Stores</span>
                    </div>
                    <div class="nav-item deepchat-item" data-page="chat">
                            <i class="fas fa-message"></i>
                            <span>Deep Chat</span>
                        </div>
                    <div class="nav-item" data-page="places">
                        <i class="fas fa-landmark"></i>
                        <span>Places</span>
                   </div>
                    <div class="nav-item" data-page="marketplace">
                        <i class="fas fa-shopping-cart"></i>
                        <span>Online</span>
                    </div>
                </div>
            </nav>
            
            <div class="menu-overlay" id="menuOverlay">
                <div class="menu-sections">
                    <div class="menu-section">
                        <div class="menu-section-title">About Us</div>
                        <div class="about-content">
                            Deep Chatbot is your premier destination for on-demand services. We connect you with skilled professionals to meet all your service needs with just a few taps.
                        </div>
                    </div>
                                        
                    <div class="menu-section">
                        <div class="menu-section-title">Connect With Us</div>
                        <div class="social-links">
                            <a href="https://nysaabhi.github.io/chat" class="social-link">
                                <i class="fab fa-facebook-f"></i>
                            </a>
                            <a href="#" class="social-link">
                                <i class="fab fa-twitter"></i>
                            </a>
                            <a href="#" class="social-link">
                                <i class="fab fa-instagram"></i>
                            </a>
                            <a href="#" class="social-link">
                                <i class="fab fa-linkedin-in"></i>
                            </a>
                        </div>
                    </div>
                    
                    <div class="menu-section">
                        <div class="menu-section-title">Our Locations</div>
                        <div class="location-links">
                            <a href="location.html" class="location-link">
                                <i class="fas fa-map-marker-alt"></i> India
                            </a>
                        </div>
                    </div>
                    
                    <div class="menu-section">
                        <div class="menu-section-title">Deep Partners</div>
                        <div class="partner-links">
                            <a href="https://www.example1.com" class="partner-link">
                                <i class="fas fa-handshake"></i> Service Provider
                            </a>
                            <a href="https://www.example2.com" class="partner-link">
                                <i class="fas fa-handshake"></i> Vendors
                            </a>
                            <a href="https://www.example3.com" class="partner-link">
                                <i class="fas fa-handshake"></i> Clients
                            </a>
                            <a href="https://www.example4.com" class="partner-link">
                                <i class="fas fa-handshake"></i> Smart Home Innovations
                            </a>
                        </div>
                    </div>
                </div>
            </div>
                
            <div class="chat-input-container">
                <div class="input-with-voice">
                    <input type="text" id="chatInput" placeholder="Search for services or ask a question..." />
                    <button class="voice-button" id="voiceButton">
                        <i class="fas fa-microphone"></i>
                    </button>
                </div>
                <button class="send-message" id="sendButton">
                    <i class="fas fa-paper-plane"></i>
                </button>
            </div>
            </div>
                
        
        
            <script src="https://cdnjs.cloudflare.com/ajax/libs/flatpickr/4.6.13/flatpickr.min.js"></script>
            <script>                        
// Complete Chat System Implementation
const searchConfig = {
  promptDatabase: {
  'where can i recharge my phone': {
    answer: 'You can recharge your phone using Paytm, PhonePe, or Google Pay.',
    clickButtons: [
      {
        label: 'Recharge',
        url: 'https://paytm.com/recharge',
        icon: 'fa-mobile'
      }
    ],
    elaborateInfo: {
      'Payment Methods': [
        'UPI payments through digital wallets',
        'Credit/Debit cards',
        'Net banking',
        'Wallet balance'
      ],
      'Process': [
        'Select operator and circle',
        'Enter mobile number',
        'Choose recharge plan',
        'Select payment method',
        'Complete payment'
      ],
      'Security': [
        'Encrypted transactions',
        'OTP verification',
        'Payment protection guarantee'
      ]
    }
  },

  'book cab': {
    answer: 'You can book a cab using Uber or Ola apps.',
    clickButtons: [
      {
        label: 'Uber',
        url: '/redirect-uber',
        icon: 'fa-car'
      },
      {
        label: 'Ola',
        url: '/redirect-ola',
        icon: 'fa-taxi'
      }
    ],
    elaborateInfo: {
      'Available Services': [
        'Mini/Sedan/SUV options',
        'Shared rides',
        'Rentals',
        'Outstation'
      ],
      'Booking Steps': [
        'Enter pickup location',
        'Choose destination',
        'Select car type',
        'Confirm booking'
      ],
      'Payment Options': [
        'Cash payment',
        'Digital wallets',
        'Cards',
        'UPI'
      ]
    }
  },

  'apply for aadhar card': {
    answer: 'You can apply for Aadhar card online or visit nearest enrollment center.',
    clickButtons: [
      {
        label: 'Apply',
        url: 'https://uidai.gov.in/my-aadhaar/appointment-booking.html',
        icon: 'fa-id-card'
      },
      {
        label: 'Find',
        url: 'https://appointments.uidai.gov.in/centerSearch',
        icon: 'fa-location-dot'
      }
    ],
    elaborateInfo: {
      'Required Documents': [
        'Proof of Identity (Passport/DL/Voter ID)',
        'Proof of Address (Utility bill/Rent agreement)',
        'Proof of Birth (Birth certificate/School certificate)',
        'Recent passport size photograph'
      ],
      'Application Steps': [
        'Book appointment online or visit center',
        'Submit required documents',
        'Biometric capture (fingerprints, iris, photograph)',
        'Receive acknowledgment slip',
        'Track application status online'
      ],
      'Important Notes': [
        'Free of cost for first time applicants',
        'Mandatory biometric update for children at 5 and 15 years',
        'SMS notifications for status updates',
        'E-Aadhaar available for download after generation'
      ]
    }
  },

  'apply for pan card': {
    answer: 'You can apply for PAN card online through NSDL or UTITSL portal.',
    clickButtons: [
      {
        label: 'Apply Online',
        url: 'https://www.onlineservices.nsdl.com/paam/endUserRegisterContact.html',
        icon: 'fa-credit-card'
      }
    ],
    elaborateInfo: {
      'Required Documents': [
        'Proof of Identity',
        'Proof of Address',
        'Proof of Date of Birth',
        'Recent passport size photograph',
        'Digital signature (for online application)'
      ],
      'Application Process': [
        'Fill online application form',
        'Upload required documents',
        'Pay application fee',
        'Schedule in-person verification if required',
        'Track application status'
      ],
      'Fee Structure': [
        'Regular application: ₹107',
        'Express delivery: Additional charges apply',
        'Digital signature charges if applicable'
      ]
    }
  },

  'open bank account': {
    answer: 'You can open a bank account online or visit nearest branch of preferred bank.',
    clickButtons: [
      {
        label: 'Compare Banks',
        url: '/compare-banks',
        icon: 'fas fa-building'
      }
    ],
    elaborateInfo: {
      'Required Documents': [
        'Valid ID proof (Aadhar/PAN/Passport)',
        'Address proof',
        'Passport size photographs',
        'Income proof (for certain accounts)'
      ],
      'Account Types': [
        'Savings Account',
        'Current Account',
        'Salary Account',
        'Fixed Deposit Account'
      ],
      'Process Steps': [
        'Choose account type and bank',
        'Fill application form',
        'Submit KYC documents',
        'Initial deposit (if required)',
        'Receive account kit'
      ],
      'Features to Compare': [
        'Minimum balance requirement',
        'Interest rates',
        'Online banking facilities',
        'Debit card features',
        'Banking charges'
      ]
    }
  },

  'file income tax returns': {
    answer: 'You can file your Income Tax Returns (ITR) online through the Income Tax portal.',
    clickButtons: [
      {
        label: 'File ITR',
        url: 'https://www.incometax.gov.in/iec/foportal',
        icon: 'fa-file-invoice-dollar'
      }
    ],
    elaborateInfo: {
      'Required Documents': [
        'Form 16 from employer',
        'Bank statements',
        'Investment proofs',
        'Property tax receipts',
        'Loan statements'
      ],
      'Filing Process': [
        'Register on Income Tax portal',
        'Choose correct ITR form',
        'Fill in tax details',
        'Upload required documents',
        'Verify using Aadhaar OTP/Digital Signature'
      ],
      'Important Dates': [
        'Financial Year: April 1 to March 31',
        'Due date for individuals: July 31',
        'Late filing fees applicable after due date'
      ],
      'Additional Information': [
        'Tax saving investments under 80C',
        'Deductions available',
        'Penalty for non-filing',
        'Tax refund process'
      ]
    }
  },

  'apply for passport': {
    answer: 'You can apply for passport online through Passport Seva portal.',
    clickButtons: [
      {
        label: 'Apply Now',
        url: 'https://www.passportindia.gov.in',
        icon: 'fa-passport'
      }
    ],
    elaborateInfo: {
      'Required Documents': [
        'Proof of Identity',
        'Proof of Address',
        'Birth Certificate',
        'Educational certificates',
        'Aadhaar card'
      ],
      'Application Steps': [
        'Register on Passport Seva portal',
        'Fill online application form',
        'Schedule appointment',
        'Pay fees online',
        'Visit PSK for verification'
      ],
      'Fee Structure': [
        'Normal Application: ₹1500',
        'Tatkal Application: ₹3500',
        'Additional charges for SMS updates'
      ],
      'Processing Time': [
        'Normal: 30-45 days',
        'Tatkal: 1-3 days',
        'Subject to police verification'
      ]
    }
  }
},

  couponDatabase: {
    keywords: ['coupon', 'coupons', 'discount', 'offer', 'code', 'deal', 'deals', 'promo'],
    brands: ['amazon', 'flipkart', 'swiggy', 'myntra', 'zomato'],
    items: {
      'amazon': [
        {
          code: 'SAVE20',
          discount: '20% OFF',
          validity: '31 Jan 2025',
          maxDiscount: '₹2000',
          brand: 'Amazon',
          category: 'E-commerce',
          description: 'Get 20% off on all products',
          icon: 'fa-shopping-cart',
          eligibility: {
            'Minimum Purchase': '₹1000',
            'Valid Categories': [
              'Electronics',
              'Fashion',
              'Home & Kitchen'
            ],
            'Payment Methods': [
              'All payment methods accepted',
              'No cash on delivery'
            ],
            'User Eligibility': [
              'All users',
              'One time per user'
            ],
            'Exclusions': [
              'Not valid on lightning deals',
              'Cannot be combined with other offers'
            ]
          }
        },
        {
          code: 'NEWUSER50',
          discount: '50% OFF',
          validity: '31 Dec 2024',
          maxDiscount: '₹500',
          brand: 'Amazon',
          category: 'E-commerce',
          description: 'First order discount for new users',
          icon: 'fa-shopping-cart',
          eligibility: {
            'Minimum Purchase': '₹500',
            'Valid Categories': ['All categories'],
            'Payment Methods': ['All payment methods'],
            'User Eligibility': ['New users only'],
            'Exclusions': ['Some premium brands excluded']
          }
        }
      ],
      'flipkart': [
        {
          code: 'FLIP30',
          discount: '30% OFF',
          validity: '30 Jan 2025',
          maxDiscount: '₹1500',
          brand: 'Flipkart',
          category: 'E-commerce',
          description: 'Special discount on electronics',
          icon: 'fa-shopping-cart',
          eligibility: {
            'Minimum Purchase': '₹3000',
            'Valid Categories': ['Electronics only'],
            'Payment Methods': ['All except COD'],
            'User Eligibility': ['All users'],
            'Exclusions': ['Not valid on Apple products']
          }
        }
      ],
      'swiggy': [
        {
          code: 'TASTE40',
          discount: '40% OFF',
          validity: '15 Jan 2025',
          maxDiscount: '₹100',
          brand: 'Swiggy',
          category: 'Food Delivery',
          description: 'Food delivery discount',
          icon: 'fa-utensils',
          eligibility: {
            'Minimum Order': '₹200',
            'Valid Time': ['All day'],
            'Payment Methods': ['All digital payments'],
            'User Eligibility': ['All users'],
            'Exclusions': ['Not valid on delivery charges']
          }
        }
      ],
      'myntra': [
        {
          code: 'FASHION25',
          discount: '25% OFF',
          validity: '28 Feb 2025',
          maxDiscount: '₹1000',
          brand: 'Myntra',
          category: 'Fashion',
          description: 'Fashion & lifestyle discount',
          icon: 'fa-tshirt',
          eligibility: {
            'Minimum Purchase': '₹1499',
            'Valid Categories': ['All Fashion Categories'],
            'Payment Methods': ['All payment methods'],
            'User Eligibility': ['All users'],
            'Exclusions': ['Not valid on luxury brands']
          }
        }
      ],
      'zomato': [
        {
          code: 'ZFIRST',
          discount: '60% OFF',
          validity: '31 Jan 2025',
          maxDiscount: '₹150',
          brand: 'Zomato',
          category: 'Food Delivery',
          description: 'New user special discount',
          icon: 'fa-hamburger',
          eligibility: {
            'Minimum Order': '₹159',
            'Valid Time': ['11 AM to 11 PM'],
            'Payment Methods': ['Online payments only'],
            'User Eligibility': ['First-time users'],
            'Exclusions': ['Not valid on dine-in']
          }
        }
      ]
    }
  },

  listDatabase: {
    'doctors': {
      keywords: ['doctor', 'doctors', 'medical', 'physician', 'clinic', 'healthcare'],
      locations: ['bhopal', 'indore', 'mumbai', 'delhi', 'bangalore'],
      items: {
        'bhopal': [
          {
            id: 'doc1',
            name: 'Dr. Rahul Sharma',
            specialty: 'Cardiologist',
            experience: '15 years',
            location: 'Arera Colony, Bhopal',
            rating: 4.8,
            consultationFee: '₹800',
            availability: 'Mon-Sat, 10:00 AM - 6:00 PM',
            touchButtons: [
              {
                label: 'Book Appointment',
                url: '/book-appointment/doc1',
                icon: 'fa-calendar-check'
              },
              {
                label: 'Video Consult',
                url: '/video-consult/doc1',
                icon: 'fa-video'
              }
            ],
            detailedInfo: {
              'Education': [
                'MBBS - AIIMS Delhi',
                'MD Cardiology - Fortis Hospital',
                'Fellowship in Interventional Cardiology'
              ],
              'Specializations': [
                'Heart Disease',
                'Cardiac Surgery',
                'Hypertension Management'
              ],
              'Languages': ['English', 'Hindi'],
              'Facilities': [
                'ECG',
                'Echo Cardiogram',
                'Stress Test'
              ]
            }
          },
          {
            id: 'doc2',
            name: 'Dr. Priya Patel',
            specialty: 'Pediatrician',
            experience: '12 years',
            location: 'MP Nagar, Bhopal',
            rating: 4.9,
            consultationFee: '₹600',
            availability: 'Mon-Sat, 9:00 AM - 5:00 PM',
            touchButtons: [
              {
                label: 'Book Appointment',
                url: '/book-appointment/doc2',
                icon: 'fa-calendar-check'
              },
              {
                label: 'Video Consult',
                url: '/video-consult/doc2',
                icon: 'fa-video'
              }
            ],
            detailedInfo: {
              'Education': [
                'MBBS - Gandhi Medical College',
                'MD Pediatrics - AIIMS Bhopal'
              ],
              'Specializations': [
                'Child Healthcare',
                'Vaccination',
                'Development Disorders'
              ],
              'Languages': ['English', 'Hindi'],
              'Facilities': [
                'Child Growth Monitoring',
                'Vaccination Services',
                'Development Assessment'
              ]
            }
          }
        ]
      }
    },
    'jewellery stores': {
      keywords: ['jewellery', 'jewelry', 'jeweler', 'jewellers', 'gold', 'diamond'],
      locations: ['mumbai', 'delhi', 'bangalore', 'pune'],
      items: {
        'mumbai': [
          {
            id: 'jew1',
            name: 'Royal Jewellers',
            type: 'Premium Jewellery Store',
            location: 'Bandra West, Mumbai',
            rating: 4.7,
            established: '1985',
            specialization: 'Diamond Jewellery',
            touchButtons: [
              {
                label: 'Book Appoinment',
                url: '/book-jeweller/jew1',
                icon: 'fa-gem'
              },
              {
                label: 'View More',
                url: '/collection/jew1',
                icon: 'fa-images'
              }
            ],
            detailedInfo: {
              'Collections': [
                'Diamond Jewellery',
                'Gold Ornaments',
                'Bridal Collection',
                'Contemporary Designs'
              ],
              'Services': [
                'Custom Design',
                'Jewellery Repair',
                'Gold Exchange',
                'Certification'
              ],
              'Payment Options': [
                'Cash',
                'Cards',
                'EMI Available',
                'Gold Exchange'
              ]
            }
          }
        ]
      }
    },
    'wedding_venues': {
      keywords: ['wedding', 'venue', 'marriage', 'banquet', 'hall'],
      locations: ['bangalore', 'mumbai', 'delhi', 'hyderabad'],
      items: {
        'bangalore': [
          {
            id: 'venue1',
            name: 'Royal Palace Gardens',
            type: 'Luxury Wedding Venue',
            location: 'Whitefield, Bangalore',
            capacity: '1000+ guests',
            rating: 4.9,
            priceRange: '₹₹₹₹',
            touchButtons: [
              {
                label: 'Check Availability',
                url: '/check-venue/venue1',
                icon: 'fa-calendar'
              },
              {
                label: 'Virtual Tour',
                url: '/tour/venue1',
                icon: 'fa-vr-cardboard'
              }
            ],
            detailedInfo: {
              'Amenities': [
                'Air Conditioned Halls',
                'Valet Parking',
                'Professional Catering',
                'Decor Services'
              ],
              'Spaces Available': [
                'Garden Area: 20,000 sq ft',
                'Indoor Hall: 15,000 sq ft',
                'Pre-function Area',
                'Dedicated Parking'
              ],
              'Services': [
                'In-house Catering',
                'Decor Team',
                'Event Planning',
                'Security Services'
              ]
            }
          }
        ]
      }
    }
  },

  researchDatabase: {
    categories: {
      hotels: {
        keywords: ['hotel', 'hotels', 'stay', 'accommodation', 'resort'],
        locations: ['bhopal', 'mumbai', 'delhi', 'indore', 'bangalore'],
        items: {
          bhopal: [
            {
              id: 'hotel1',
              name: 'Taj Lakefront Bhopal',
              rating: 4.8,
              priceRange: '₹₹₹₹',
              location: 'Lake View, Bhopal',
              amenities: ['Free WiFi', 'Swimming Pool', 'Spa', 'Restaurant'],
              touchButtons: [
                {
                  label: 'Book Now',
                  url: 'https://tajhotels.com',
                  icon: 'fa-calendar-check'
                },
                {
                  label: 'View Photos',
                  url: '/hotel-photos/hotel1',
                  icon: 'fa-images'
                }
              ],
              detailedInfo: {
                'Address': ['Lake View Road, Bhopal, MP'],
                'Contact': ['+91 755 123 4567'],
                'Facilities': [
                  '24/7 Room Service',
                  'Conference Hall',
                  'Gymnasium',
                  'Free Parking'
                ],
                'Reviews': [
                  '⭐️⭐️⭐️⭐️⭐️ "Amazing stay with a beautiful lake view!"',
                  '⭐️⭐️⭐️⭐️ "Great service and delicious food."'
                ]
              }
            },
            {
              id: 'hotel2',
              name: 'Courtyard by Marriott',
              rating: 4.7,
              priceRange: '₹₹₹',
              location: 'MP Nagar, Bhopal',
              amenities: ['Free WiFi', 'Fitness Center', 'Bar', 'Restaurant'],
              touchButtons: [
                {
                  label: 'Book Now',
                  url: 'https://marriott.com',
                  icon: 'fa-calendar-check'
                },
                {
                  label: 'View Photos',
                  url: '/hotel-photos/hotel2',
                  icon: 'fa-images'
                }
              ],
              detailedInfo: {
                'Address': ['MP Nagar Zone II, Bhopal, MP'],
                'Contact': ['+91 755 234 5678'],
                'Facilities': [
                  'Business Center',
                  'Swimming Pool',
                  'Spa',
                  'Free Parking'
                ],
                'Reviews': [
                  '⭐️⭐️⭐️⭐️⭐️ "Luxurious and comfortable!"',
                  '⭐️⭐️⭐️⭐️ "Excellent staff and service."'
                ]
              }
            }
          ],
          mumbai: [
            // Similar structure for Mumbai hotels
          ]
        }
      },
      tourism: {
        keywords: ['tourism', 'tourist', 'spots', 'attractions', 'places'],
        locations: ['mumbai', 'delhi', 'bhopal', 'indore', 'bangalore'],
        items: {
          mumbai: [
            {
              id: 'tourism1',
              name: 'Gateway of India',
              rating: 4.9,
              location: 'Apollo Bandar, Mumbai',
              description: 'Iconic monument overlooking the Arabian Sea.',
              touchButtons: [
                {
                  label: 'View Details',
                  url: '/tourism-details/tourism1',
                  icon: 'fa-info-circle'
                },
                {
                  label: 'Book Tour',
                  url: '/book-tour/tourism1',
                  icon: 'fa-ticket-alt'
                }
              ],
              detailedInfo: {
                'Timings': ['Open 24 hours'],
                'Entry Fee': ['Free'],
                'Nearby Attractions': [
                  'Taj Mahal Palace Hotel',
                  'Elephanta Caves',
                  'Marine Drive'
                ],
                'Tips': [
                  'Best time to visit: Early morning or evening',
                  'Avoid weekends for less crowd'
                ]
              }
            }
          ]
        }
      },
      wifi: {
        keywords: ['wifi', 'internet', 'hotspot', 'cafe', 'coffee'],
        locations: ['indore', 'mumbai', 'delhi', 'bhopal', 'bangalore'],
        items: {
          indore: [
            {
              id: 'wifi1',
              name: 'Café Coffee Day',
              rating: 4.5,
              location: 'Vijay Nagar, Indore',
              description: 'Reliable WiFi with great coffee.',
              touchButtons: [
                {
                  label: 'View Menu',
                  url: '/cafe-menu/wifi1',
                  icon: 'fa-coffee'
                },
                {
                  label: 'Directions',
                  url: '/directions/wifi1',
                  icon: 'fa-map-marker-alt'
                }
              ],
              detailedInfo: {
                'Timings': ['8:00 AM - 11:00 PM'],
                'WiFi Speed': ['Up to 50 Mbps'],
                'Facilities': [
                  'Air Conditioning',
                  'Power Outlets',
                  'Comfortable Seating'
                ],
                'Reviews': [
                  '⭐️⭐️⭐️⭐️ "Great place to work with fast WiFi."',
                  '⭐️⭐️⭐️⭐️ "Friendly staff and good coffee."'
                ]
              }
            }
          ]
        }
      },
      busAgencies: {
        keywords: ['bus', 'agency', 'travel', 'transport', 'bus service'],
        locations: ['delhi', 'mumbai', 'bhopal', 'indore', 'bangalore'],
        items: {
          delhi: [
            {
              id: 'bus1',
              name: 'Delhi Transport Corporation',
              rating: 4.3,
              location: 'Kashmere Gate, Delhi',
              description: 'Reliable and affordable bus services.',
              touchButtons: [
                {
                  label: 'Book Ticket',
                  url: '/book-bus/bus1',
                  icon: 'fa-ticket-alt'
                },
                {
                  label: 'View Routes',
                  url: '/bus-routes/bus1',
                  icon: 'fa-route'
                }
              ],
              detailedInfo: {
                'Contact': ['+91 11 2389 1234'],
                'Services': [
                  'AC Buses',
                  'Non-AC Buses',
                  'Night Services'
                ],
                'Fare': [
                  'Starting from ₹10 for non-AC',
                  'Starting from ₹20 for AC'
                ],
                'Reviews': [
                  '⭐️⭐️⭐️⭐️ "Affordable and punctual service."',
                  '⭐️⭐️⭐️ "Clean buses and good frequency."'
                ]
              }
            }
          ]
        }
      }
    }
  },

  medicineDatabase: {
    keywords: ['medicine', 'drug', 'pill', 'tablet', 'capsule', 'syrup', 'injection', 'dose', 'side effects', 'uses'],
    medicines: {
      'paracetamol': {
        name: 'Paracetamol',
        brandNames: ['Crocin', 'Calpol', 'Dolo'],
        type: 'Analgesic and Antipyretic',
        uses: [
          'Relieves mild to moderate pain (headache, toothache, muscle pain)',
          'Reduces fever'
        ],
        dosage: {
          adults: '500mg to 1000mg every 4-6 hours (max 4000mg per day)',
          children: '10-15mg per kg body weight every 4-6 hours'
        },
        sideEffects: [
          'Nausea',
          'Stomach pain',
          'Allergic reactions (rare)'
        ],
        precautions: [
          'Do not exceed the recommended dose',
          'Avoid alcohol while taking paracetamol',
          'Consult a doctor if fever persists for more than 3 days'
        ],
        interactions: [
          'Alcohol: Increases risk of liver damage',
          'Warfarin: May increase bleeding risk'
        ],
        storage: 'Store at room temperature, away from moisture and heat',
        touchButtons: [
          {
            label: 'Buy Online',
            url: 'https://pharmacyonline.com/paracetamol',
            icon: 'fa-shopping-cart'
          },
          {
            label: 'View Alternatives',
            url: '/medicine-alternatives/paracetamol',
            icon: 'fa-pills'
          }
        ]
      },
      'omeprazole': {
        name: 'Omeprazole',
        brandNames: ['Omez', 'Losec', 'Prilosec'],
        type: 'Proton Pump Inhibitor (PPI)',
        uses: [
          'Treats acid reflux and heartburn',
          'Heals stomach ulcers',
          'Manages Zollinger-Ellison syndrome'
        ],
        dosage: {
          adults: '20mg once daily before meals',
          children: 'Not recommended for children under 12'
        },
        sideEffects: [
          'Headache',
          'Nausea',
          'Diarrhea',
          'Abdominal pain'
        ],
        precautions: [
          'Take on an empty stomach',
          'Avoid long-term use without medical advice',
          'Consult a doctor if symptoms persist'
        ],
        interactions: [
          'Clopidogrel: May reduce effectiveness',
          'Ketoconazole: May reduce absorption'
        ],
        storage: 'Store at room temperature, away from moisture',
        touchButtons: [
          {
            label: 'Buy Online',
            url: 'https://pharmacyonline.com/omeprazole',
            icon: 'fa-shopping-cart'
          },
          {
            label: 'View Alternatives',
            url: '/medicine-alternatives/omeprazole',
            icon: 'fa-pills'
          }
        ]
      }
    }
  },

  realEstateDatabase: {
    keywords: ['property', 'real estate', 'house', 'apartment', 'rent', 'buy', 'BHK', 'sq ft', 'near', 'school', 'hospital', 'park', 'shopping'],
    locations: ['new delhi', 'bangalore', 'bhopal', 'mumbai', 'hyderabad'],
    properties: {
      'new delhi': [
        {
          id: 'prop1',
          type: 'Apartment',
          bhk: 3,
          price: '₹4,80,000',
          size: '1200 sq ft',
          location: 'Saket, New Delhi',
          amenities: ['Swimming Pool', 'Gym', 'Parking', '24/7 Security'],
          nearbyFacilities: {
            schools: ['Delhi Public School (1 km)', 'Springdales School (2 km)'],
            hospitals: ['Max Hospital (3 km)', 'Apollo Hospital (5 km)'],
            parks: ['Deer Park (2 km)', 'Lodhi Garden (4 km)'],
            shopping: ['Select Citywalk Mall (1 km)', 'Anupam Market (3 km)']
          },
          touchButtons: [
            {
              label: 'View Photos',
              url: '/property-photos/prop1',
              icon: 'fa-images'
            },
            {
              label: 'Contact Agent',
              url: '/contact-agent/prop1',
              icon: 'fa-phone'
            }
          ]
        },
        {
          id: 'prop2',
          type: 'Villa',
          bhk: 4,
          price: '₹1,20,00,000',
          size: '3000 sq ft',
          location: 'Vasant Vihar, New Delhi',
          amenities: ['Private Garden', 'Swimming Pool', 'Gym', 'Parking'],
          nearbyFacilities: {
            schools: ['The Shri Ram School (2 km)', 'Vasant Valley School (3 km)'],
            hospitals: ['Fortis Hospital (4 km)', 'Medanta Hospital (6 km)'],
            parks: ['Nehru Park (1 km)', 'Rose Garden (3 km)'],
            shopping: ['Ambience Mall (5 km)', 'DLF Promenade (7 km)']
          },
          touchButtons: [
            {
              label: 'View Photos',
              url: '/property-photos/prop2',
              icon: 'fa-images'
            },
            {
              label: 'Contact Agent',
              url: '/contact-agent/prop2',
              icon: 'fa-phone'
            }
          ]
        }
      ],
      'bangalore': [
        {
          id: 'prop3',
          type: 'Apartment',
          bhk: 2,
          price: '₹75,00,000',
          size: '1000 sq ft',
          location: 'Koramangala, Bangalore',
          amenities: ['Clubhouse', 'Gym', 'Parking', '24/7 Security'],
          nearbyFacilities: {
            schools: ['Inventure Academy (2 km)', 'Ryan International School (3 km)'],
            hospitals: ['Manipal Hospital (4 km)', 'Narayana Health (5 km)'],
            parks: ['Agara Lake (1 km)', 'Cubbon Park (6 km)'],
            shopping: ['Forum Mall (2 km)', 'Phoenix Marketcity (8 km)']
          },
          touchButtons: [
            {
              label: 'View Photos',
              url: '/property-photos/prop3',
              icon: 'fa-images'
            },
            {
              label: 'Contact Agent',
              url: '/contact-agent/prop3',
              icon: 'fa-phone'
            }
          ]
        }
      ],
      'bhopal': [
        {
          id: 'prop4',
          type: 'Apartment',
          bhk: 3,
          price: '₹50,00,000',
          size: '1500 sq ft',
          location: 'Arera Colony, Bhopal',
          amenities: ['Swimming Pool', 'Gym', 'Parking', '24/7 Security'],
          nearbyFacilities: {
            schools: ['Campion School (1 km)', 'St. Joseph School (2 km)'],
            hospitals: ['Bhopal Hospital (3 km)', 'Chirayu Hospital (4 km)'],
            parks: ['Van Vihar National Park (2 km)', 'Kerwa Dam (10 km)'],
            shopping: ['DB City Mall (5 km)', 'Aashima Mall (8 km)']
          },
          touchButtons: [
            {
              label: 'View Photos',
              url: '/property-photos/prop4',
              icon: 'fa-images'
            },
            {
              label: 'Contact Agent',
              url: '/contact-agent/prop4',
              icon: 'fa-phone'
            }
          ]
        }
      ]
    }
  },

  qaDatabase: {
    'how do i book a service': {
      answer: 'To book a service, click on the category, select a service, choose a package, and fill out the booking form. You can also call us directly at +91 9669181789 for immediate assistance.',
      category: 'Booking',
      keywords: ['book', 'service', 'booking', 'schedule', 'appointment']
    },
    'what services do you offer': {
      answer: 'We offer a wide range of home and occasion services including Plumbing, Electrical, Carpentry, Painting, Appliance Repair, AC/Heating Repair, Cleaning Services, and Skills Training.',
      category: 'Services',
      keywords: ['services', 'offer', 'available', 'provide', 'types']
    },
    'hey': {
      answer: 'Hello! How can I assist you today?',
      category: 'Greeting',
      keywords: ['hey', 'hi', 'hello', 'greetings']
    },
    'hello': {
      answer: 'Hi there! What can I help you with?',
      category: 'Greeting',
      keywords: ['hello', 'hi', 'hey']
    },
    'how are you': {
      answer: 'I am just a program, but I am here to help! How about you?',
      category: 'General',
      keywords: ['how', 'are', 'you', 'doing']
    },
    'good morning': {
      answer: 'Good morning! How can I assist you today?',
      category: 'Greeting',
      keywords: ['good', 'morning']
    },
    'good afternoon': {
      answer: 'Good afternoon! How may I help you?',
      category: 'Greeting',
      keywords: ['good', 'afternoon']
    },
    'good evening': {
      answer: 'Good evening! What can I do for you today?',
      category: 'Greeting',
      keywords: ['good', 'evening']
    },
    'what is your name': {
      answer: 'I am your assistant. How can I help you?',
      category: 'General',
      keywords: ['your', 'name']
    },
    'who are you': {
      answer: 'I am your assistant, here to assist you with your queries.',
      category: 'General',
      keywords: ['who', 'are', 'you']
    },
    'what can you do': {
      answer: 'I can help you book services, answer your queries, and assist with information about our offerings.',
      category: 'General',
      keywords: ['what', 'can', 'do']
    },
    'thank you': {
      answer: 'You’re welcome! Let me know if you need any further assistance.',
      category: "Polite",
      keywords: ['thank', 'you', 'thanks']
    },
    'bye': {
      answer: 'Goodbye! Have a great day!',
      category: 'Polite',
      keywords: ['bye', 'goodbye', 'see', 'later']
    },
    'do you offer emergency services': {
      answer: 'Yes, we do offer emergency services. Please call us at +91 9669181789 for immediate assistance.',
      category: 'Services',
      keywords: ['emergency', 'services', 'urgent', 'immediate']
    },
    'how do i cancel a booking': {
      answer: 'To cancel a booking, please log into your account, go to your bookings, and select the cancel option. You can also contact our support team.',
      category: 'Booking',
      keywords: ['cancel', 'booking', 'remove', 'appointment']
    },
    'do you have a mobile app': {
      answer: 'Yes, we have a mobile app available for download on Android and iOS platforms. Search for our app in your store.',
      category: 'General',
      keywords: ['mobile', 'app', 'application', 'download']
    },
    'how can i give feedback': {
      answer: 'You can provide feedback through our website or mobile app under the feedback section. We value your input!',
      category: 'Feedback',
      keywords: ['feedback', 'review', 'suggestion', 'input']
    },
    'do you offer discounts': {
      answer: 'Yes, we have seasonal and promotional discounts. Please check our website or subscribe to our newsletter for updates.',
      category: 'Services',
      keywords: ['discounts', 'offers', 'promotions', 'deals']
    },
    'where are you located': {
      answer: 'We are an online platform but operate across multiple cities. Contact us for location-specific services.',
      category: 'General',
      keywords: ['location', 'where', 'address', 'based']
    },
    'can i reschedule a booking': {
      answer: 'Yes, you can reschedule your booking through your account or by contacting our support team.',
      category: 'Booking',
      keywords: ['reschedule', 'change', 'booking', 'appointment']
    },
    'do you provide 24/7 support': {
      answer: 'Yes, our support team is available 24/7 to assist you.',
      category: 'Greeting',
      keywords: ['24/7', 'support', 'available', 'hours']
    },
    // ... Add additional entries in a similar structure
  },

  mathKeywords: ['calculate', 'solve', 'what is', 'find', 'area', 'perimeter', 'volume', 'divide', 'multiply', '+', '-', '*', '/', '=', 'equation'],
  
  fallbackResponses: [
    "I apologize, but I couldn't find an exact match for your query. Could you please rephrase your question?",
    "I'm not sure about that specific question. Would you like to know about our available services or booking process instead?",
    "I couldn't find a direct answer to your question. Would you like to speak with a customer service representative?"
  ],

  processRealEstateQuery(query) {
    const normalizedQuery = query.toLowerCase();
    const words = normalizedQuery.split(' ');

    // Check if the query contains real estate-related keywords
    const hasRealEstateKeyword = this.realEstateDatabase.keywords.some(keyword =>
      normalizedQuery.includes(keyword.toLowerCase())
    );

    if (!hasRealEstateKeyword) {
      return null; // Skip real estate processing if no relevant keywords are found
    }

    // Extract location, type, and filters from query
    let location = null;
    let type = null;
    let bhk = null;
    let priceRange = null;
    let nearFacility = null;

    // Check for location
    for (const loc of this.realEstateDatabase.locations) {
      if (normalizedQuery.includes(loc.toLowerCase())) {
        location = loc;
        break;
      }
    }

    // Check for property type (e.g., apartment, villa)
    if (normalizedQuery.includes('apartment')) {
      type = 'Apartment';
    } else if (normalizedQuery.includes('villa')) {
      type = 'Villa';
    }

    // Check for BHK (e.g., 2BHK, 3BHK)
    const bhkMatch = normalizedQuery.match(/(\d+)\s*BHK/);
    if (bhkMatch) {
      bhk = parseInt(bhkMatch[1]);
    }

    // Check for price range (e.g., under 500,000)
    const priceMatch = normalizedQuery.match(/(under|below|less than)\s*(\d+,\d+|\d+)/);
    if (priceMatch) {
      priceRange = priceMatch[2].replace(/,/g, '');
    }

    // Check for nearby facilities (e.g., near schools, near hospitals)
    if (normalizedQuery.includes('near')) {
      if (normalizedQuery.includes('school')) {
        nearFacility = 'schools';
      } else if (normalizedQuery.includes('hospital')) {
        nearFacility = 'hospitals';
      } else if (normalizedQuery.includes('park')) {
        nearFacility = 'parks';
      } else if (normalizedQuery.includes('shopping')) {
        nearFacility = 'shopping';
      }
    }

    // Filter properties based on query
    if (location && this.realEstateDatabase.properties[location]) {
      let properties = this.realEstateDatabase.properties[location];

      if (type) {
        properties = properties.filter(prop => prop.type === type);
      }
      if (bhk) {
        properties = properties.filter(prop => prop.bhk === bhk);
      }
      if (priceRange) {
        properties = properties.filter(prop => {
          const propPrice = parseInt(prop.price.replace(/[^0-9]/g, ''));
          return propPrice <= priceRange;
        });
      }
      if (nearFacility) {
        properties = properties.filter(prop => prop.nearbyFacilities[nearFacility].length > 0);
      }

      if (properties.length > 0) {
        return this.generateRealEstateResponse(location, properties);
      }
    }

    return this.generateGeneralRealEstateResponse();
  },

  generateRealEstateResponse(location, properties) {
    const locationTitle = this.capitalizeFirst(location);

    const listHTML = properties.map(prop => `
      <div class="property-item" onclick="toggleDetailedInfo(this)">
        <div class="property-header">
          <h3>${prop.type} - ${prop.bhk}BHK</h3>
          <div class="property-price">${prop.price}</div>
        </div>
        <div class="property-content">
          <p><strong>Location:</strong> ${prop.location}</p>
          <p><strong>Size:</strong> ${prop.size}</p>
          <div class="amenities">
            ${prop.amenities.map(amenity => `<span class="amenity">${amenity}</span>`).join('')}
          </div>
        </div>
        <div class="property-actions" onclick="event.stopPropagation()">
          ${prop.touchButtons.map(btn => `
            <button class="touch-button" data-url="${btn.url}">
              <i class="fas ${btn.icon}"></i> ${btn.label}
            </button>
          `).join('')}
        </div>
        <div class="detailed-info hidden">
          <div class="info-section">
            <h4>Nearby Facilities</h4>
            <ul>
              ${Object.entries(prop.nearbyFacilities).map(([facility, items]) => `
                <li><strong>${this.capitalizeFirst(facility)}:</strong> ${items.join(', ')}</li>
              `).join('')}
            </ul>
          </div>
        </div>
      </div>
    `).join('');

    return `
      <div class="real-estate-container">
        <h2>Properties in ${locationTitle}</h2>
        ${listHTML}
      </div>
    `;
  },

  generateGeneralRealEstateResponse() {
    return `
      <div class="real-estate-container">
        <h2>Real Estate Information</h2>
        <p>Here are some common queries you can ask:</p>
        <ul>
          <li>"Find a 3BHK in New Delhi under ₹50,00,000"</li>
          <li>"Show me rental options near schools in Bhopal"</li>
          <li>"What are the current property rates in Bangalore?"</li>
        </ul>
      </div>
    `;
  },
  
  processResearchQuery(query) {
    const normalizedQuery = query.toLowerCase();
    const words = normalizedQuery.split(' ');

    // Extract category and location from query
    let category = null;
    let location = null;

    // Check for category keywords
    for (const [cat, data] of Object.entries(this.researchDatabase.categories)) {
      if (data.keywords.some(keyword => words.includes(keyword))) {
        category = cat;
        break;
      }
    }

    // Check for location
    if (category) {
      const locations = this.researchDatabase.categories[category].locations;
      location = locations.find(loc => words.includes(loc.toLowerCase()));
    }

    if (category && location) {
      const items = this.researchDatabase.categories[category].items[location];
      if (items && items.length > 0) {
        return this.generateResearchResponse(category, location, items);
      }
    }

    return null;
  },

  generateResearchResponse(category, location, items) {
    const categoryTitle = this.capitalizeFirst(category);
    const locationTitle = this.capitalizeFirst(location);

    const listHTML = items.map(item => `
      <div class="research-item" onclick="toggleDetailedInfo(this)">
        <div class="item-header">
          <h3>${item.name}</h3>
          <div class="rating">★ ${item.rating}</div>
        </div>
        <div class="item-content">
          <p>${item.location}</p>
          ${item.description ? `<p>${item.description}</p>` : ''}
          ${item.amenities ? `
            <div class="amenities">
              ${item.amenities.map(amenity => `<span class="amenity">${amenity}</span>`).join('')}
            </div>
          ` : ''}
        </div>
        <div class="item-actions" onclick="event.stopPropagation()">
          ${item.touchButtons.map(btn => `
            <button class="touch-button" data-url="${btn.url}">
              <i class="fas ${btn.icon}"></i> ${btn.label}
            </button>
          `).join('')}
        </div>
        <div class="detailed-info hidden">
          ${Object.entries(item.detailedInfo).map(([section, items]) => `
            <div class="info-section">
              <h4>${section}</h4>
              <ul>
                ${items.map(item => `<li>${item}</li>`).join('')}
              </ul>
            </div>
          `).join('')}
        </div>
      </div>
    `).join('');

    return `
      <div class="research-container">
        <h2>Top ${items.length} ${categoryTitle} in ${locationTitle}</h2>
        ${listHTML}
      </div>
    `;
  },

  processMedicineQuery(query) {
    const normalizedQuery = query.toLowerCase();
    const words = normalizedQuery.split(' ');

    // Check if the query contains medicine-related keywords
    const hasMedicineKeyword = this.medicineDatabase.keywords.some(keyword =>
      words.includes(keyword.toLowerCase())
    );

    if (hasMedicineKeyword) {
      // Search for medicine by name
      for (const [medicineName, data] of Object.entries(this.medicineDatabase.medicines)) {
        if (normalizedQuery.includes(medicineName.toLowerCase())) {
          return this.generateMedicineResponse(data);
        }
      }

      // If no specific medicine is found, show a general response
      return this.generateGeneralMedicineResponse();
    }

    return null;
  },

  generateMedicineResponse(medicineData) {
    return `
      <div class="medicine-response">
        <div class="medicine-header">
          <h2>${medicineData.name}</h2>
          <div class="medicine-type">${medicineData.type}</div>
        </div>
        <div class="medicine-content">
          <div class="medicine-section">
            <h3>Uses</h3>
            <ul>
              ${medicineData.uses.map(use => `<li>${use}</li>`).join('')}
            </ul>
          </div>
          <div class="medicine-section">
            <h3>Dosage</h3>
            <ul>
              <li><strong>Adults:</strong> ${medicineData.dosage.adults}</li>
              <li><strong>Children:</strong> ${medicineData.dosage.children}</li>
            </ul>
          </div>
          <div class="medicine-section">
            <h3>Side Effects</h3>
            <ul>
              ${medicineData.sideEffects.map(effect => `<li>${effect}</li>`).join('')}
            </ul>
          </div>
          <div class="medicine-section">
            <h3>Precautions</h3>
            <ul>
              ${medicineData.precautions.map(precaution => `<li>${precaution}</li>`).join('')}
            </ul>
          </div>
          <div class="medicine-section">
            <h3>Interactions</h3>
            <ul>
              ${medicineData.interactions.map(interaction => `<li>${interaction}</li>`).join('')}
            </ul>
          </div>
          <div class="medicine-section">
            <h3>Storage</h3>
            <p>${medicineData.storage}</p>
          </div>
        </div>
        <div class="medicine-actions">
          ${medicineData.touchButtons.map(btn => `
            <button class="touch-button" data-url="${btn.url}">
              <i class="fas ${btn.icon}"></i> ${btn.label}
            </button>
          `).join('')}
        </div>
      </div>
    `;
  },

  generateGeneralMedicineResponse() {
    return `
      <div class="medicine-response">
        <h2>Medicines Information</h2>
        <p>Here are some common medicines you can ask about:</p>
        <ul>
          <li>Paracetamol</li>
          <li>Omeprazole</li>
          <li>Ibuprofen</li>
          <li>Amoxicillin</li>
        </ul>
        <p>Try asking: "Tell me about paracetamol" or "What are the side effects of omeprazole?"</p>
      </div>
    `;
  },

  processList(query) {
    const normalizedQuery = query.toLowerCase();
    const words = normalizedQuery.split(' ');
    
    for (const [category, data] of Object.entries(this.listDatabase)) {
      const hasKeyword = data.keywords.some(keyword => 
        words.includes(keyword.toLowerCase())
      );
      
      const location = data.locations.find(loc => 
        words.includes(loc.toLowerCase())
      );
      
      if (hasKeyword && location) {
        return this.generateListResponse(category, location, data.items[location]);
      }
    }
    
    return null;
  },

  generateListResponse(category, location, items) {
    if (!items || items.length === 0) {
      return `No ${category} found in ${location}.`;
    }

    const listHTML = items.map(item => `
        <div class="list-item" onclick="toggleDetailedInfo(this)">
            <div class="item-header">
                <h3>${item.name}</h3>
                <div class="rating">★ ${item.rating}</div>
            </div>
            
            <div class="item-content">
                <p>${item.location}</p>
                ${this.generateItemSpecifics(item)}
            </div>
            
            <div class="item-actions" onclick="event.stopPropagation()">
                ${item.touchButtons.map(btn => `
                    <button class="touch-button" data-url="${btn.url}">
                        <i class="fas ${btn.icon}"></i> ${btn.label}
                    </button>
                `).join('')}
            </div>
            
            <div class="detailed-info hidden">
                ${Object.entries(item.detailedInfo).map(([section, items]) => `
                    <div class="info-section">
                        <h4>${section}</h4>
                        <ul>
                            ${items.map(item => `<li>${item}</li>`).join('')}
                        </ul>
                    </div>
                `).join('')}
            </div>
        </div>
    `).join('');

    return `
        <div class="list-container">
            <h2>${this.capitalizeFirst(category)} in ${this.capitalizeFirst(location)}</h2>
            ${listHTML}
        </div>
    `;
},

  capitalizeFirst(str) {
    return str.charAt(0).toUpperCase() + str.slice(1);
  },

  generateItemSpecifics(item) {
    const specifics = [];
    if (item.specialty) specifics.push(`Specialty: ${item.specialty}`);
    if (item.experience) specifics.push(`Experience: ${item.experience}`);
    if (item.type) specifics.push(`Type: ${item.type}`);
    if (item.capacity) specifics.push(`Capacity: ${item.capacity}`);
    if (item.priceRange) specifics.push(`Price Range: ${item.priceRange}`);
    if (item.consultationFee) specifics.push(`Consultation Fee: ${item.consultationFee}`);
    if (item.availability) specifics.push(`Availability: ${item.availability}`);
    
    return specifics.map(spec => `<p class="item-specific">${spec}</p>`).join('');
  }
};

class EnhancedMathSolver {
  constructor() {
    this.basicOperators = {
      'plus': '+',
      'add': '+',
      'sum': '+',
      'minus': '-',
      'subtract': '-',
      'difference': '-',
      'times': '*',
      'multiply': '*',
      'product': '*',
      'divided by': '/',
      'divide': '/',
      'over': '/',
      'power': '^',
      'squared': '^2',
      'cubed': '^3'
    };

    // Word problem patterns
    this.wordProblemPatterns = [
      {
        type: 'interest',
        patterns: [
          /(\d+)\s*Rs?\s+at\s+(\d+(?:\.\d+)?)\s*%\s+interest\s+(monthly|per\s+annum|yearly|annually).*?(\d+)\s+(month|year|months|years)/i,
          /lends?\s+(\d+)\s*Rs?\s+.*?(\d+(?:\.\d+)?)\s*%\s+interest\s+(monthly|per\s+annum|yearly|annually).*?(\d+)\s+(month|year|months|years)/i
        ],
        solve: (matches) => {
          const principal = parseFloat(matches[1]);
          const rate = parseFloat(matches[2]);
          const timeUnit = matches[3].toLowerCase();
          const time = parseFloat(matches[4]);
          const periodUnit = matches[5].toLowerCase();
          
          let interest = 0;
          let totalAmount = 0;
          
          if (timeUnit.includes('month') || timeUnit === 'monthly') {
            interest = (principal * rate * time) / 100;
          } else {
            const timeInYears = periodUnit.includes('month') ? time / 12 : time;
            interest = (principal * rate * timeInYears) / 100;
          }
          
          totalAmount = principal + interest;
          
          return {
            result: interest,
            explanation: `Principal Amount: ₹${principal}\nInterest Rate: ${rate}%\nTime Period: ${time} ${periodUnit}\nInterest Earned: ₹${interest.toFixed(2)}\nTotal Amount: ₹${totalAmount.toFixed(2)}`
          };
        }
      },
      {
        type: 'speed',
        patterns: [
          /travels?\s+(\d+)\s*(km|kilometer|kilometers)\s+in\s+(\d+)\s*(hour|hours|hr|hrs)/i,
          /(\d+)\s*(km|kilometer|kilometers)\s+.*?(\d+)\s*(hour|hours|hr|hrs).*?average\s+speed/i
        ],
        solve: (matches) => {
          const distance = parseFloat(matches[1]);
          const time = parseFloat(matches[3]);
          const speed = distance / time;
          
          return {
            result: speed,
            explanation: `Distance: ${distance} km\nTime: ${time} hours\nAverage Speed: ${speed} km/h`
          };
        }
      },
      {
        type: 'percentage',
        patterns: [
          /(\d+)\s*(boys|girls).*?(\d+)\s*(boys|girls).*?percentage/i,
          /what\s+percentage\s+.*?(\d+)\s+.*?(\d+)/i
        ],
        solve: (matches) => {
          let part, whole;
          if (matches.length > 3) {
            const firstNum = parseFloat(matches[1]);
            const secondNum = parseFloat(matches[3]);
            const firstType = matches[2];
            const secondType = matches[4];
            
            whole = firstNum + secondNum;
            part = firstType === 'girls' ? firstNum : secondNum;
          } else {
            part = parseFloat(matches[1]);
            whole = parseFloat(matches[2]);
          }
          
          const percentage = (part / whole) * 100;
          
          return {
            result: percentage,
            explanation: `Part: ${part}\nTotal: ${whole}\nPercentage: ${percentage.toFixed(2)}%`
          };
        }
      },
      {
        type: 'remaining',
        patterns: [
          /(\d+)\s*(liters?|L)\s+.*?(\d+)\s*(liters?|L)\s+.*?(left|remaining|unused)/i,
          /(\d+)\s*(liters?|L)\s+.*?(\d+)\s*(liters?|L)\s+.*?used/i
        ],
        solve: (matches) => {
          const total = parseFloat(matches[1]);
          const used = parseFloat(matches[3]);
          const remaining = total - used;
          
          return {
            result: remaining,
            explanation: `Total Amount: ${total} liters\nUsed Amount: ${used} liters\nRemaining Amount: ${remaining} liters`
          };
        }
      }
    ];

    this.mathFunctions = {
      'sqrt': Math.sqrt,
      'square root': Math.sqrt,
      'cube root': (x) => Math.pow(x, 1/3),
      'absolute': Math.abs,
      'sin': Math.sin,
      'cos': Math.cos,
      'tan': Math.tan,
      'log': Math.log10,
      'ln': Math.log,
      'factorial': this.factorial,
      'percentage': (x) => x / 100
    };
  }

  detectMathQuery(query) {
    const normalizedQuery = query.toLowerCase();
    
    // Check for word problems first
    for (const problemType of this.wordProblemPatterns) {
      for (const pattern of problemType.patterns) {
        if (pattern.test(normalizedQuery)) {
          return true;
        }
      }
    }
    
    // Then check for regular math expressions
    const hasMathFunction = Object.keys(this.mathFunctions).some(func => 
      normalizedQuery.includes(func)
    );
    const hasOperatorKeyword = Object.keys(this.basicOperators).some(op => 
      normalizedQuery.includes(op)
    );
    const hasNumbers = /\d+(\.\d+)?/.test(normalizedQuery);
    const hasAdvancedPattern = /(?:solve|calculate|evaluate|find|what is|compute)/i.test(normalizedQuery);
    
    return hasMathFunction || hasOperatorKeyword || (hasNumbers && hasAdvancedPattern);
  }

  solveMathProblem(query) {
    const normalizedQuery = query.toLowerCase();
    
    // Try to solve as a word problem first
    for (const problemType of this.wordProblemPatterns) {
      for (const pattern of problemType.patterns) {
        const matches = normalizedQuery.match(pattern);
        if (matches) {
          const solution = problemType.solve(matches);
          return this.formatWordProblemResponse(problemType.type, solution);
        }
      }
    }
    
    // If not a word problem, use existing math solving logic
    try {
      const parsedExpression = this.parseExpression(normalizedQuery);
      const result = eval(parsedExpression.replace(/\^/g, '**'));
      return this.formatArithmeticResponse(parsedExpression, result);
    } catch (e) {
      return this.handleError(query);
    }
  }

  formatWordProblemResponse(type, solution) {
    return `📝 Problem Solution:\n\n${solution.explanation}`;
  }

  formatArithmeticResponse(expression, result) {
    return `🔢 Result: ${result}`;
  }

  handleError(query) {
    return `❌ Unable to process the mathematical expression: "${query}"\nPlease check the format and try again.`;
  }

  factorial(n) {
    if (n === 0 || n === 1) return 1;
    return n * this.factorial(n - 1);
  }

  parseExpression(query) {
    let normalizedQuery = query.toLowerCase()
      .replace(/what is|calculate|solve|find|evaluate|compute/g, '')
      .trim();

    Object.entries(this.basicOperators).forEach(([word, symbol]) => {
      normalizedQuery = normalizedQuery.replace(new RegExp(word, 'g'), symbol);
    });

    return normalizedQuery;
  }
}

class ChatSystem {
  constructor() {
    this.messageHistory = [];
    this.typingTimeout = null;
    this.similarityThreshold = 0.5;
    this.mathSolver = new EnhancedMathSolver();
    this.hasUserChatted = false; // Track if the user has started chatting
  }

  initialize() {
    this.chatInput = document.getElementById('chatInput');
    this.chatMessages = document.getElementById('chatMessages');
    this.sendButton = document.getElementById('sendButton');
    this.setupEventListeners();
    this.displayWelcomeMessage();
  }

  setupEventListeners() {
    this.sendButton.addEventListener('click', () => this.handleUserInput());
    this.chatInput.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') {
        e.preventDefault();
        this.handleUserInput();
      }
    });
    this.chatInput.addEventListener('input', () => this.handleTypingIndicator());
  }

  handleTypingIndicator() {
    clearTimeout(this.typingTimeout);
    const typingIndicator = document.getElementById('typingIndicator');
    
    if (!typingIndicator) {
      const indicator = document.createElement('div');
      indicator.id = 'typingIndicator';
      indicator.className = 'message bot-message typing-indicator';
      indicator.innerHTML = '<div class="dots"><span>.</span><span>.</span><span>.</span></div>';
      this.chatMessages.appendChild(indicator);
    }

    this.typingTimeout = setTimeout(() => {
      const indicator = document.getElementById('typingIndicator');
      if (indicator) indicator.remove();
    }, 1000);
  }

  displayWelcomeMessage() {
    const welcomeMessage = `
      <div id="welcomeMessage" class="message bot-message welcome-message">
        <div class="welcome-avatar"><i class="fas fa-message"></i></div>
        <div class="welcome-content">
          <p>👋 Hello! I'm your On-Demand services assistant.</p>
          <p>I can help you find:</p>
          <ul>
            <li>Doctors in your city</li>
            <li>Jewellery stores</li>
            <li>Wedding venues</li>
            <li>And more!</li>
          </ul>
          <p>Try asking me something like "Show me doctors in Bhopal" or "Find jewellery stores in Mumbai"</p>
        </div>
      </div>`;
    this.chatMessages.innerHTML = welcomeMessage;
  }

  async handleUserInput() {
    const query = this.chatInput.value.trim();
    if (!query) return;

    // Remove the welcome message if it's the first user message
    if (!this.hasUserChatted) {
      this.hasUserChatted = true;
      const welcomeMessage = document.getElementById('welcomeMessage');
      if (welcomeMessage) {
        welcomeMessage.classList.add('fade-out');
        setTimeout(() => welcomeMessage.remove(), 300); // Wait for fade-out animation
      }
    }

    this.displayMessage(query, 'user');
    this.chatInput.value = '';

    await this.simulateProcessing();
    const response = this.processQuery(query);
    this.displayMessage(response, 'bot');
    
    this.messageHistory.push({ query, response, timestamp: new Date() });
    this.scrollToBottom();
    
    this.setupClickButtons();
    this.setupTouchButtons();
  }

  setupClickButtons() {
      document.querySelectorAll('.click-button').forEach(button => {
          button.addEventListener('click', () => {
              const url = button.getAttribute('data-url');
              if (url) window.location.href = url;
          });
      });
  }

  setupTouchButtons() {
      document.querySelectorAll('.touch-button').forEach(button => {
          button.addEventListener('click', () => {
              const url = button.getAttribute('data-url');
              if (url) window.location.href = url;
          });
      });
  }

  processQuery(query) {
    // Try to process as a coupon query first
    const couponResponse = this.processCouponQuery(query);
    if (couponResponse) return couponResponse;

    // Try to process as a list query
    const listResponse = searchConfig.processList(query);
    if (listResponse) return listResponse;

      // Try to process as a research query
  const researchResponse = searchConfig.processResearchQuery(query);
  if (researchResponse) return researchResponse;

      // Try to process as a medicine query
      const medicineResponse = searchConfig.processMedicineQuery(query);
    if (medicineResponse) return medicineResponse;

        // Try to process as a real estate query
        const realEstateResponse = searchConfig.processRealEstateQuery(query);
    if (realEstateResponse) return realEstateResponse;


    // Try to process as a prompt query
    const promptResponse = this.processPromptQuery(query);
    if (promptResponse) return promptResponse;

    // Check for math queries
    if (this.mathSolver.detectMathQuery(query)) {
      return this.mathSolver.solveMathProblem(query);
    }

    // Search in QA database
    return this.searchDatabase(query);
  }

  setupClickButtons() {
    document.querySelectorAll('.click-button').forEach(button => {
      button.addEventListener('click', () => {
        const url = button.getAttribute('data-url');
        if (url) window.location.href = url;
      });
    });
  }

  processPromptQuery(query) {
    const promptData = searchConfig.promptDatabase[query.toLowerCase()];
    if (!promptData) return null;
    return this.createPromptResponse(promptData);
  }

   createPromptResponse(promptData) {
  return `
    <div class="prompt-response" onclick="toggleElaborateInfo(this)">
      <div class="answer">${promptData.answer}</div>
      <div class="click-buttons">
        ${promptData.clickButtons.map(btn => `
          <button class="click-button" data-url="${btn.url}" onclick="event.stopPropagation()">
            <i class="fas ${btn.icon}"></i> ${btn.label}
          </button>
        `).join('')}
      </div>
      <div class="elaborate-info hidden">
        ${Object.entries(promptData.elaborateInfo).map(([section, items]) => `
          <div class="elaborate-section">
            <h3>${section}</h3>
            <ul>${items.map(item => `<li>${item}</li>`).join('')}</ul>
          </div>
        `).join('')}
      </div>
    </div>
  `;
}

processCouponQuery(query) {
    const normalizedQuery = query.toLowerCase();
    const words = normalizedQuery.split(' ');
    
    const hasCouponKeyword = searchConfig.couponDatabase.keywords.some(keyword => 
      words.includes(keyword.toLowerCase())
    );
    
    if (!hasCouponKeyword) return null;
    
    const brand = searchConfig.couponDatabase.brands.find(brand => 
      words.includes(brand.toLowerCase())
    );
    
    if (brand) {
      return this.generateCouponResponse([brand]);
    } else {
      return this.generateCouponResponse(searchConfig.couponDatabase.brands);
    }
  }

  generateCouponResponse(brands) {
    let couponsHTML = '';
    
    brands.forEach(brand => {
      const coupons = searchConfig.couponDatabase.items[brand];
      if (coupons && coupons.length > 0) {
        couponsHTML += coupons.map(coupon => `
          <div class="coupon-item" onclick="toggleCouponInfo(this)">
            <div class="coupon-header">
              <div class="brand-info">
                <i class="fas ${coupon.icon}"></i>
                <span>${coupon.brand}</span>
              </div>
              <div class="discount-tag">${coupon.discount}</div>
            </div>
            
            <div class="coupon-content">
              <p class="description">${coupon.description}</p>
              <div class="code-container">
                <code class="coupon-code">${coupon.code}</code>
                <button class="copy-button" onclick="event.stopPropagation(); copyCouponCode('${coupon.code}')">
                  <i class="fas fa-copy"></i>
                </button>
              </div>
              <div class="validity">Valid till: ${coupon.validity}</div>
            </div>
            
            <div class="coupon-info hidden">
              ${Object.entries(coupon.eligibility).map(([section, items]) => `
                <div class="info-section">
                  <h4>${section}</h4>
                  ${Array.isArray(items) 
                    ? `<ul>${items.map(item => `<li>${item}</li>`).join('')}</ul>`
                    : `<p>${items}</p>`
                  }
                </div>
              `).join('')}
            </div>
          </div>
        `).join('');
      }
    });

    return `
      <div class="coupons-container">
        <h2>Available Coupons</h2>
        ${couponsHTML}
      </div>
    `;
  }

  searchDatabase(query) {
    const normalizedQuery = query.toLowerCase();
    const words = normalizedQuery.split(/\s+/);
    const results = [];

    for (const [question, data] of Object.entries(searchConfig.qaDatabase)) {
      if (normalizedQuery === question.toLowerCase()) {
        results.push({ similarity: 1, data });
        continue;
      }

      const keywordMatch = data.keywords?.some(keyword => 
        words.includes(keyword.toLowerCase())
      );
      if (keywordMatch) {
        results.push({ similarity: 0.8, data });
        continue;
      }

      const similarity = this.calculateSimilarity(normalizedQuery, question.toLowerCase());
      if (similarity > this.similarityThreshold) {
        results.push({ similarity, data });
      }
    }

    if (results.length > 0) {
      results.sort((a, b) => b.similarity - a.similarity);
      return results[0].data.answer;
    }

    return this.getRandomFallbackResponse();
  }

  calculateSimilarity(str1, str2) {
    const words1 = str1.split(/\s+/);
    const words2 = str2.split(/\s+/);
    const set1 = new Set(words1);
    const set2 = new Set(words2);
    const intersection = new Set([...set1].filter(x => set2.has(x)));
    const union = new Set([...set1, ...set2]);
    const jaccardSimilarity = intersection.size / union.size;
    
    let orderSimilarity = 0;
    words1.forEach((word, index) => {
      const pos2 = words2.indexOf(word);
      if (pos2 !== -1) {
        orderSimilarity += 1 - Math.abs(index - pos2) / Math.max(words1.length, words2.length);
      }
    });
    
    return (jaccardSimilarity * 0.7) + (orderSimilarity / words1.length * 0.3);
  }

  getRandomFallbackResponse() {
    const responses = searchConfig.fallbackResponses;
    return responses[Math.floor(Math.random() * responses.length)];
  }

  displayMessage(content, type) {
    const messageDiv = document.createElement('div');
    messageDiv.className = `message ${type}-message`;
    
    if (type === 'bot') {
      messageDiv.innerHTML = `
        <div class="message-avatar"><i class="fas fa-comments"></i></div>
        <div class="message-content">${content}</div>`;
    } else {
      messageDiv.innerHTML = `
        <div class="message-content">${content}</div>`;
    }
    
    this.chatMessages.appendChild(messageDiv);
  }

  async simulateProcessing() {
    const typingIndicator = document.createElement('div');
    typingIndicator.className = 'message bot-message typing-indicator';
    typingIndicator.innerHTML = '<div class="dots"><span>.</span><span>.</span><span>.</span></div>';
    this.chatMessages.appendChild(typingIndicator);
    this.scrollToBottom();
    await new Promise(resolve => setTimeout(resolve, 1000));
    typingIndicator.remove();
  }

  scrollToBottom() {
    this.chatMessages.scrollTop = this.chatMessages.scrollHeight;
  }
}

// Menu System Implementation
function setupMenu() {
  const menuButton = document.getElementById('menuButton');
  const menuOverlay = document.getElementById('menuOverlay');
  const menuItems = document.querySelectorAll('.menu-item');
  const chatMessages = document.getElementById('chatMessages');
  
  const closeButton = document.createElement('button');
  closeButton.innerHTML = '<i class="fas fa-long-arrow-alt-left"></i>';
  closeButton.className = 'menu-close-button';
  menuOverlay.appendChild(closeButton);

  function toggleMenu(show) {
      menuOverlay.style.display = show ? 'block' : 'none';
      menuOverlay.classList.toggle('fade-in', show);
      document.body.style.overflow = show ? 'hidden' : '';
  }

  menuButton.addEventListener('click', () => toggleMenu(true));
  closeButton.addEventListener('click', () => toggleMenu(false));
  menuOverlay.addEventListener('click', (e) => {
      if (e.target === menuOverlay) toggleMenu(false);
  });

  menuItems.forEach(item => {
      item.addEventListener('click', () => {
          const selectedItem = item.textContent.trim();
          const message = document.createElement('div');
          message.className = 'message bot-message';
          message.innerHTML = `
              <div class="message-avatar"><i class="fas fa-comments"></i></div>
              <div class="message-content">You selected: ${selectedItem}. How can I assist you further?</div>
          `;
          chatMessages.appendChild(message);
          chatMessages.scrollTop = chatMessages.scrollHeight;
          toggleMenu(false);
      });
  });

  document.addEventListener('keydown', (e) => {
      if (e.key === 'Escape') toggleMenu(false);
  });
}

        // Voice Recognition Class
        class VoiceRecognition {
            constructor() {
                this.recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
                this.recognition.continuous = false;
                this.recognition.interimResults = false;
                this.recognition.lang = 'en-US';

                this.recognition.onresult = (event) => {
                    const transcript = event.results[0][0].transcript;
                    this.handleVoiceInput(transcript);
                };

                this.recognition.onerror = (event) => {
                    console.error('Speech recognition error:', event.error);
                    this.showToast('Error: ' + event.error);
                };

                this.recognition.onend = () => {
                    this.recognition.stop();
                };
            }

            start() {
                this.recognition.start();
                this.showToast('Listening...');
            }

            handleVoiceInput(transcript) {
                const chatInput = document.getElementById('chatInput');
                chatInput.value = transcript;
                chatSystem.handleUserInput();
            }

            showToast(message) {
                const toast = document.createElement('div');
                toast.className = 'toast';
                toast.textContent = message;
                document.body.appendChild(toast);

                setTimeout(() => {
                    toast.classList.add('show');
                    setTimeout(() => {
                        toast.classList.remove('show');
                        setTimeout(() => toast.remove(), 300);
                    }, 2000);
                }, 100);
            }
        }

        // Initialize Voice Recognition
        const voiceRecognition = new VoiceRecognition();

        // Add Event Listener to Voice Button
        document.getElementById('voiceButton').addEventListener('click', () => {
            voiceRecognition.start();
        });

// Utility function to toggle detailed information
function toggleDetailedInfo(element) {
  const infoPanel = element.closest('.list-item').querySelector('.detailed-info');
  infoPanel.classList.toggle('hidden');
}

function toggleElaborateInfo(promptResponse) {
    const infoPanel = promptResponse.querySelector('.elaborate-info');
    infoPanel.classList.toggle('hidden');
}


// Utility functions
function toggleCouponInfo(element) {
  const infoPanel = element.querySelector('.coupon-info');
  infoPanel.classList.toggle('hidden');
}

function copyCouponCode(code) {
  navigator.clipboard.writeText(code).then(() => {
    showToast('Coupon code copied!');
  }).catch(err => {
    showToast('Failed to copy code');
  });
}

function showToast(message) {
  const toast = document.createElement('div');
  toast.className = 'toast';
  toast.textContent = message;
  document.body.appendChild(toast);
  
  setTimeout(() => {
    toast.classList.add('show');
    setTimeout(() => {
      toast.classList.remove('show');
      setTimeout(() => toast.remove(), 300);
    }, 2000);
  }, 100);
}

document.addEventListener('DOMContentLoaded', function() {
  const chatSystem = new ChatSystem();
  chatSystem.initialize();
  setupMenu();

  if (typeof flatpickr !== 'undefined') {
      flatpickr("#dateInput", {
          minDate: "today",
          maxDate: new Date().fp_incr(30),
          dateFormat: "Y-m-d"
      });

      flatpickr("#timeInput", {
          enableTime: true,
          noCalendar: true,
          dateFormat: "H:i",
          minTime: "09:00",
          maxTime: "18:00",
          minuteIncrement: 30
      });
  }
});

const style = document.createElement('style');
style.textContent = `
  .typing-indicator {
      padding: 20px;
  }

  .typing-indicator .dots {
      display: inline-flex;
      gap: 4px;
  }

  .typing-indicator .dots span {
      width: 8px;
      height: 8px;
      background: #666;
      border-radius: 50%;
      animation: bounce 1.4s infinite ease-in-out;
  }

  .typing-indicator .dots span:nth-child(1) { animation-delay: -0.32s; }
  .typing-indicator .dots span:nth-child(2) { animation-delay: -0.16s; }

  @keyframes bounce {
      0%, 80%, 100% { transform: scale(0); }
      40% { transform: scale(1); }
  }

    .message {
    display: flex; 
    gap: 10px; 
    max-width: 100%; 
    margin-bottom: 16px; 
    animation: messageSlide 0.3s ease-out; 
    }

    .bot-message { 
  align-self: flex-start; 
}
   
   .user-message {
        background: #lalala;
        color: white;
        margin-left: auto;
    }

    .message-avatar {
        width: 30px;
        height: 30px;
        background: #lalala;
        color: #FFD700;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .welcome-message ul {
        margin: 10px 0;
        padding-left:20px;
    }

    .fade-out {
  animation: fadeOut 0.3s ease-out forwards;
    }

   @keyframes fadeOut {
   from { opacity: 1; }
   to { opacity: 0; transform: translateY(-20px); }
   }

    .menu-close-button {
        position: absolute;
        top: 15px;
        right: 15px;
        background: #1a1a1f;
        border: none;
        color: #FFD700;
        font-size: 24px;
        cursor: pointer;
        padding: 5px;
        z-index: 1001;
    }

    .menu-close-button:hover {
        color: #FFD700;
    }

.prompt-response {
  background: #lalala;
  color: white;
  margin-left: auto;
  position: relative;  /* Add relative positioning for absolute child */
}

    .click-buttons {
        display: flex;
        align-items: center;
        gap: 8px;
        margin-top: 10px;
        height: 40px;
        pointer-events: none; /* Prevent action buttons from triggering detailed info */
    }

    .click-button {
        pointer-events: auto; /* Re-enable clicks for action buttons */
        padding: 8px 16px;
        background: linear-gradient(to bottom, #2563eb, #1d4ed8);
        color: white;
        border: none;
        border-radius: 6px;
        cursor: pointer;
        display: flex;
        align-items: center;
        gap: 8px;
        font-weight: 500;
        transition: all 0.2s ease;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        height: 40px;
        line-height: 40px;
    }

    .click-button:hover {
        background: linear-gradient(to bottom, #1d4ed8, #1e40af);
        transform: translateY(-1px);
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.15);
    }

    .elaborate-info {
        margin-top: 15px;
        padding: 15px;
        background: #lalala;
        border-radius: 4px;
        clear: both;
        animation: fadeIn 0.3s ease-out;
    }

    .hidden {
        display: none;
    }

    .elaborate-section {
        margin-bottom: 15px;
    }

    .elaborate-section h3 {
        margin-bottom: 8px;
        color: #fff;
    }

    .elaborate-section ul {
        margin: 0;
        padding-left: 20px;
    }

/* Root Variables 
-------------------------------------------------- */
:root {
  --container-bg: #1a1a1f;
  --item-bg: #232328;
  --accent-color: #FFD700;
  --text-primary: rgba(255, 255, 255, 0.9);
  --text-secondary: rgba(255, 255, 255, 0.8);
  --border-color: rgba(255, 255, 255, 0.1);
  --button-gradient: linear-gradient(135deg, #2563eb, #1d4ed8);
  --button-gradient-active: linear-gradient(135deg, #1d4ed8, #1e40af);
  --shadow-sm: 0 4px 6px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 8px 16px rgba(0, 0, 0, 0.15);
}

/* Container Layout
-------------------------------------------------- */
.list-container {
  background: transparent;
  color: white;
  margin: 0 auto;
  padding: clamp(16px, 4vw, 24px);
  border-radius: 12px;
  max-width: 1200px;
  width: 100%;
  box-sizing: border-box;
}

/* List Items
-------------------------------------------------- */
.list-item {
  background: var(--item-bg);
  border-radius: 10px;
  padding: clamp(16px, 3vw, 24px);
  margin-bottom: 16px;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  border: 1px solid var(--border-color);
  position: relative;
  overflow: hidden;
  width: 100%;
  box-sizing: border-box;
}

/* Header Section
-------------------------------------------------- */
.item-header {
  display: grid;
  grid-template-columns: 1fr auto;
  gap: 12px;
  align-items: center;
  margin-bottom: 16px;
  width: 100%;
}

.item-title {
  font-size: clamp(1.1rem, 2.5vw, 1.25rem);
  font-weight: 600;
  color: var(--accent-color);
  margin: 0;
  line-height: 1.3;
  word-wrap: break-word;
}

.item-meta {
  display: flex;
  align-items: center;
  gap: 12px;
  flex-wrap: wrap;
}

/* Content Area
-------------------------------------------------- */
.item-content {
  margin-bottom: 16px;
  line-height: 1.6;
  color: var(--text-primary);
  font-size: clamp(0.9rem, 2vw, 1rem);
  word-wrap: break-word;
  overflow-wrap: break-word;
  width: 100%;
}

/* Tags and Ratings
-------------------------------------------------- */
.rating {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  color: var(--accent-color);
  font-weight: 600;
  padding: 4px 8px;
  background: rgba(255, 215, 0, 0.1);
  border-radius: 6px;
  white-space: nowrap;
}

.item-specific {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-bottom: 16px;
  width: 100%;
}

.tag {
  background: rgba(255, 255, 255, 0.1);
  padding: 4px 12px;
  border-radius: 16px;
  font-size: clamp(0.8rem, 1.8vw, 0.9rem);
  color: var(--text-secondary);
}

/* Detailed Information Section
-------------------------------------------------- */
.detailed-info {
  margin-top: 16px;
  padding: clamp(12px, 3vw, 16px);
  background: rgba(0, 0, 0, 0.2);
  border-radius: 8px;
  border: 1px solid rgba(255, 255, 255, 0.05);
  animation: slideDown 0.3s ease-out;
  width: 100%;
  box-sizing: border-box;
}

.info-section {
  margin-bottom: 20px;
  width: 100%;
}

.info-section:last-child {
  margin-bottom: 0;
}

.info-section h3 {
  font-size: 1.1rem;
  color: var(--accent-color);
  margin-bottom: 12px;
  font-weight: 600;
}

.info-section ul {
  margin: 0;
  padding-left: 20px;
  list-style: none;
  width: 100%;
}

.info-section li {
  position: relative;
  padding-left: 16px;
  margin-bottom: 8px;
  line-height: 1.5;
  color: var(--text-primary);
  word-wrap: break-word;
}

.info-section li::before {
  content: '•';
  position: absolute;
  left: 0;
  color: var(--accent-color);
}

/* Action Buttons
-------------------------------------------------- */
.item-actions {
  display: flex;
  gap: 8px;
  flex-wrap: nowrap;
  width: 100%;
  justify-content: space-between;
  box-sizing: border-box;
}

.touch-button {
  padding: 8px 12px;
  background: var(--button-gradient);
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 6px;
  font-weight: 500;
  transition: transform 0.2s ease;
  flex: 1;
  min-width: 100px;
  text-align: center;
  justify-content: center;
  font-size: 14px;
}

/* Animations
-------------------------------------------------- */
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

@keyframes loading {
  0% { transform: translateX(-100%); }
  100% { transform: translateX(100%); }
}

/* Desktop-Specific Styles
-------------------------------------------------- */
@media (hover: hover) {
  .list-item:hover {
    transform: translateY(-2px);
    box-shadow: var(--shadow-lg);
    border-color: rgba(255, 215, 0, 0.3);
  }
  
  .touch-button:hover {
    transform: translateY(-1px);
    box-shadow: 0 4px 12px rgba(37, 99, 235, 0.2);
  }
}

/* Mobile-Specific Styles
-------------------------------------------------- */
@media (max-width: 480px) {
  .list-container {
    padding: 8px;
    margin: 0;
    max-width: none;
    border-radius: 0;
  }
  
  .list-item {
    padding: 16px;
    margin: 8px auto;
    width: 240px;
    max-width: none;
  }
  
  /* Rest of your mobile styles remain the same */
  .item-header {
    grid-template-columns: 1fr;
    gap: 8px;
    margin-bottom: 12px;
  }
  
  .item-meta {
    justify-content: flex-start;
  }
  
  .detailed-info {
    margin-top: 12px;
    padding: 12px;
  }
}
  
  .info-section h3 {
    font-size: 1rem;
  }
  
  .info-section li {
    font-size: 0.9rem;
    padding-left: 12px;
    margin-bottom: 6px;
  }
  
  .item-actions {
    flex-direction: column;
    gap: 8px;
  }
  
  .touch-button {
    width: 100%;
    min-width: unset;
    padding: 10px;
  }
}

/* Touch Device Interactions
-------------------------------------------------- */
@media (hover: none) {
  .touch-button:active {
    transform: translateY(1px);
    background: var(--button-gradient-active);
  }
}

/* Loading States
-------------------------------------------------- */
.loading {
  position: relative;
  overflow: hidden;
}

.loading::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255, 255, 255, 0.1),
    transparent
  );
  animation: loading 1.5s infinite;
}

  .coupons-container {
    display: flex;
    flex-direction: column;
    gap: 16px;
    width: 100%;
    max-width: 600px;
  }

  .coupon-item {
    background: #1a1a1f;
    border-radius: 12px;
    padding: 16px;
    cursor: pointer;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
    border: 1px solid rgba(255, 255, 255, 0.1);
  }

  .coupon-item:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
  }

  .coupon-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 12px;
  }

  .brand-info {
    display: flex;
    align-items: center;
    gap: 8px;
    font-weight: 500;
  }

  .brand-info i {
    font-size: 20px;
    color: #FFD700;
  }

  .discount-tag {
    background: linear-gradient(135deg, #2563eb, #1d4ed8);
    color: white;
    padding: 4px 8px;
    margin-left: 18px;
    border-radius: 4px;
    font-weight: 600;
  }

  .coupon-content {
    margin-bottom: 12px;
  }

  .description {
    margin-bottom: 8px;
    color: #fff;
  }

  .code-container {
    display: flex;
    align-items: center;
    gap: 8px;
    margin: 12px 0;
    background: rgba(255, 255, 255, 0.05);
    padding: 8px;
    border-radius: 6px;
  }

  .coupon-code {
    font-family: monospace;
    font-size: 16px;
    letter-spacing: 1px;
    color: #FFD700;
    background: transparent;
  }

  .copy-button {
    background: transparent;
    border: none;
    color: #FFD700;
    cursor: pointer;
    padding: 4px 8px;
    transition: transform 0.2s ease;
  }

  .copy-button:hover {
    transform: scale(1.1);
  }

  .validity {
    font-size: 0.9em;
    color: #888;
  }

  .toast {
    position: fixed;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%) translateY(100px);
    background: rgba(0, 0, 0, 0.8);
    color: white;
    padding: 12px 24px;
    border-radius: 8px;
    transition: transform 0.3s ease;
    z-index: 1000;
  }

  .toast.show {
    transform: translateX(-50%) translateY(0);
  }

  .coupon-info {
    margin-top: 16px;
    padding-top: 16px;
    border-top: 1px solid rgba(255, 255, 255, 0.1);
  }

  .coupon-info .info-section {
    margin-bottom: 12px;
  }

  .coupon-info h4 {
    font-size: 14.9px;
    margin-left: 0px;
    color: #FFD700;
    margin-bottom: 8px;
  }

  .coupon-info ul {
    font-size: 14px;
    margin-left: 0px;
    list-style: none;
    padding-left: 0;
  }

  .coupon-info li {
    color: #fff;
    margin-bottom: 4px;
    position: relative;
    padding-left: 20px;
  }

  .coupon-info li:before {
    content: '•';
    position: absolute;
    left: 0;
    color: #FFD700;
  }

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }

  @keyframes messageSlide {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
  }

  /* Research Container */
.research-container {
  background: #1a1a1f;
  border-radius: 12px;
  padding: 20px;
  margin: 16px 0;
}

.research-container h2 {
  font-size: 1.5rem;
  color: #FFD700;
  margin-bottom: 20px;
  text-align: center;
}

/* Research Item */
.research-item {
  width: 240px;
  background: #232328;
  border-radius: 10px;
  padding: 16px;
  margin-bottom: 16px;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  cursor: pointer;
}

.research-item:hover {
  transform: translateY(-4px);
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.3);
  border-color: rgba(255, 215, 0, 0.3);
}

/* Item Header */
.item-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.item-header h3 {
  font-size: 1.2rem;
  color: #FFD700;
  margin: 0;
}

.rating {
  background: rgba(255, 215, 0, 0.1);
  color: #FFD700;
  padding: 6px 12px;
  border-radius: 20px;
  font-weight: 600;
  font-size: 0.9rem;
}

/* Item Content */
.item-content {
  color: rgba(255, 255, 255, 0.9);
  font-size: 0.95rem;
  line-height: 1.6;
}

.item-content p {
  margin: 8px 0;
}

.amenities {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-top: 12px;
}

.amenity {
  background: rgba(255, 255, 255, 0.1);
  color: rgba(255, 255, 255, 0.9);
  padding: 6px 12px;
  border-radius: 20px;
  font-size: 0.85rem;
}

/* Item Actions */
.item-actions {
  display: flex;
  gap: 8px;
  margin-top: 16px;
}

.touch-button {
  flex: 1;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: white;
  border: none;
  border-radius: 8px;
  padding: 10px 16px;
  font-size: 0.9rem;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  transition: all 0.3s ease;
}

.touch-button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(37, 99, 235, 0.3);
}

.touch-button i {
  font-size: 1rem;
}

/* Detailed Info */
.detailed-info {
  margin-top: 16px;
  padding-top: 16px;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  animation: fadeIn 0.3s ease;
}

.detailed-info h4 {
  font-size: 1.1rem;
  color: #FFD700;
  margin-bottom: 12px;
}

.detailed-info ul {
  list-style: none;
  padding-left: 0;
  margin: 0;
}

.detailed-info li {
  color: rgba(255, 255, 255, 0.9);
  margin-bottom: 8px;
  padding-left: 20px;
  position: relative;
}

.detailed-info li::before {
  content: '•';
  position: absolute;
  left: 0;
  color: #FFD700;
  font-size: 1.2rem;
  line-height: 1;
}

/* Animations */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateX(-20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

/* Responsive Design */
@media (max-width: 768px) {
  .research-container {
    padding: 12px;
  }

  .research-item {
    padding: 12px;
  }

  .item-header h3 {
    font-size: 1.1rem;
  }

  .touch-button {
    font-size: 0.85rem;
    padding: 8px 12px;
  }
}

/* Medicine Response */
.medicine-response {
  background: #232328;
  border-radius: 12px;
  padding: 20px;
  margin: 16px 0;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.medicine-header {
  margin-bottom: 20px;
}

.medicine-header h2 {
  font-size: 1.5rem;
  color: #FFD700;
  margin: 0;
}

.medicine-type {
  color: rgba(255, 255, 255, 0.8);
  font-size: 0.9rem;
  margin-top: 4px;
}

.medicine-section {
  margin-bottom: 16px;
}

.medicine-section h3 {
  font-size: 1.1rem;
  color: #FFD700;
  margin-bottom: 8px;
}

.medicine-section ul {
  list-style: none;
  padding-left: 20px;
  margin: 0;
}

.medicine-section li {
  color: rgba(255, 255, 255, 0.9);
  margin-bottom: 6px;
  position: relative;
}

.medicine-section li::before {
  content: '•';
  position: absolute;
  left: -15px;
  color: #FFD700;
  font-size: 1.2rem;
  line-height: 1;
}

.medicine-actions {
  display: flex;
  gap: 8px;
  margin-top: 20px;
}

.touch-button {
  flex: 1;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: white;
  border: none;
  border-radius: 8px;
  padding: 10px 16px;
  font-size: 0.9rem;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  transition: all 0.3s ease;
}

.touch-button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(37, 99, 235, 0.3);
}

.touch-button i {
  font-size: 1rem;
}

/* Real Estate Container */
.real-estate-container {
  background: #232328;
  border-radius: 12px;
  padding: 20px;
  margin: 16px 0;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.real-estate-container h2 {
  font-size: 1.5rem;
  color: #FFD700;
  margin-bottom: 20px;
  text-align: center;
}

/* Property Item */
.property-item {
  background: #1a1a1f;
  border-radius: 10px;
  padding: 16px;
  margin-bottom: 16px;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  cursor: pointer;
}

.property-item:hover {
  transform: translateY(-4px);
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.3);
  border-color: rgba(255, 215, 0, 0.3);
}

/* Property Header */
.property-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.property-header h3 {
  font-size: 1.2rem;
  color: #FFD700;
  margin: 0;
}

.property-price {
  background: rgba(255, 215, 0, 0.1);
  color: #FFD700;
  padding: 6px 12px;
  border-radius: 20px;
  font-weight: 600;
  font-size: 0.9rem;
}

/* Property Content */
.property-content {
  color: rgba(255, 255, 255, 0.9);
  font-size: 0.95rem;
  line-height: 1.6;
}

.property-content p {
  margin: 8px 0;
}

.amenities {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-top: 12px;
}

.amenity {
  background: rgba(255, 255, 255, 0.1);
  color: rgba(255, 255, 255, 0.9);
  padding: 6px 12px;
  border-radius: 20px;
  font-size: 0.85rem;
}

/* Property Actions */
.property-actions {
  display: flex;
  gap: 8px;
  margin-top: 16px;
}

.touch-button {
  flex: 1;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: white;
  border: none;
  border-radius: 8px;
  padding: 10px 16px;
  font-size: 0.9rem;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  transition: all 0.3s ease;
}

.touch-button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(37, 99, 235, 0.3);
}

.touch-button i {
  font-size: 1rem;
}

/* Detailed Info */
.detailed-info {
  margin-top: 16px;
  padding-top: 16px;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  animation: fadeIn 0.3s ease;
}

.detailed-info h4 {
  font-size: 1.1rem;
  color: #FFD700;
  margin-bottom: 12px;
}

.detailed-info ul {
  list-style: none;
  padding-left: 0;
  margin: 0;
}

.detailed-info li {
  color: rgba(255, 255, 255, 0.9);
  margin-bottom: 8px;
  padding-left: 20px;
  position: relative;
}

.detailed-info li::before {
  content: '•';
  position: absolute;
  left: 0;
  color: #FFD700;
  font-size: 1.2rem;
  line-height: 1;
}
`;

document.head.appendChild(style);

                const townsData = [
  {
    name: "Mumbai",
    image: "https://i.pinimg.com/originals/2c/72/40/2c72408b797d4841b6fe6c395183ba35.png",
    quote: "The town of dreams where ambitions take flight and opportunities never sleep"
  },
  {
    name: "Delhi",
    image: "https://www.mistay.in/travel-blog/content/images/2020/07/travel-4813658_1920.jpg",
    quote: "Where ancient heritage meets modern aspirations in perfect harmony"
  },
  {
    name: "Jaipur",
    image: "https://www.tripsavvy.com/thmb/Afl1v6bgmGid9kPfseymDiAYWa0=/3595x2397/filters:no_upscale():max_bytes(150000):strip_icc()/GettyImages-518387310-04a30994bfb1461bb8000f1864ca1fc5.jpg",
    quote: "Pink Town, where royal heritage colors modern dreams"
  }
];

function showTownsOverlay() {
  let overlay = document.getElementById('townsOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'townsOverlay';
    overlay.className = 'towns-overlay';
    document.body.appendChild(overlay);
  }
  overlay.innerHTML = `
    <div class="towns-content">
      <div class="towns-header">
        <h2>Cities We Serve</h2>
        <button class="close-towns" onclick="hideTownsOverlay()">
          <i class="fas fa-long-arrow-alt-left"></i>
        </button>
      </div>
      <div class="towns-grid">
        ${townsData.map(town => `
          <div class="town-card" onclick="selectTown('${town.name}')">
            <div class="town-image-container">
              <img src="${town.image}" alt="${town.name}" class="town-image">
              <div class="town-overlay"></div>
            </div>
            <div class="town-info">
              <h3 class="town-name">${town.name}</h3>
              <p class="town-quote">${town.quote}</p>
            </div>
          </div>
        `).join('')}
      </div>
    </div>
  `;
  setTimeout(() => overlay.classList.add('active'), 10);

  const style = document.createElement('style');
  style.textContent = `
.towns-overlay {
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  bottom: 60px;
  background:  #000000;
  z-index: 1000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  overflow-y: auto;
}

.towns-overlay.active {
  opacity: 1;
  visibility: visible;
}

.towns-content {
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

.towns-header {
  padding: 8px 16px;
  background: rgba(26, 26, 31, 0.95);
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 30;
  border-bottom: 2px solid var(--primary-color);
  backdrop-filter: blur(10px);
  height: 65px;
  /* Flexbox for horizontal layout */
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 5px;
}

.towns-header h2 {
  color: var(--primary-color);
  margin: 0;
  font-size: 1.5em;
  flex-grow: 0;
}

.close-towns {
  background: none;
  border: none;
  color: var(--primary-color);
  font-size: 1.5em;
  cursor: pointer;
  padding: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.towns-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  animation: fadeInUp 0.5s ease-out;
  margin-top: 10px; /* Ensure content starts below the header */
}

.town-card {
  border-radius: 12px;
  overflow: hidden;
  background: rgba(26, 26, 31, 0.98);
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
}

.town-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
}

.town-image-container {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
}

.town-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.town-card:hover .town-image {
  transform: scale(1.1);
}

.town-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

.town-info {
  padding: 20px;
  position: relative;
  z-index: 1;
}

.town-name {
  color: var(--text-light);
  margin: 0 0 10px 0;
  font-size: 1.4em;
}

.town-quote {
  color: #cccccc;
  font-size: 0.9em;
  line-height: 1.4;
  margin: 0;
}

    .list-container {
      padding: 15px;
      background: #f8f9fa;
      border-radius: 8px;
    }

    .list-item {
      background: white;
      border-radius: 8px;
      padding: 15px;
      margin-bottom: 15px;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
      transition: all 0.3s ease;
    }

    .list-item:hover {
      transform: translateY(-2px);
      box-shadow: 0 4px 8px rgba(0,0,0,0.15);
    }

    .item-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 10px;
    }

    .rating {
      color: #ffd700;
      font-weight: bold;
    }

    .item-content {
      margin-bottom: 15px;
    }

    .item-specific {
      font-size: 0.9em;
      color: #666;
      margin: 5px 0;
    }

    .item-actions {
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
    }

    .action-button {
      padding: 8px 16px;
      background: linear-gradient(to bottom, #2563eb, #1d4ed8);
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 8px;
      font-weight: 500;
      transition: all 0.2s ease;
    }

    .action-button:hover {
      background: linear-gradient(to bottom, #1d4ed8, #1e40af);
      transform: translateY(-1px);
    }

    .info-button {
      background: #6c757d;
      color: white;
      border: none;
      border-radius: 6px;
      padding: 8px 16px;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 8px;
      transition: all 0.2s ease;
    }

    .info-button:hover {
      background: #5a6268;
    }

    .detailed-info {
      margin-top: 15px;
      padding: 15px;
      background: #f8f9fa;
      border-radius: 4px;
      animation: fadeIn 0.3s ease-out;
    }

    .hidden {
      display: none;
    }

    .info-section {
      margin-bottom: 15px;
    }

    .info-section h4 {
      margin-bottom: 8px;
      color: #333;
    }

    .info-section ul {
      margin: 0;
      padding-left: 20px;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }
  
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

@media (max-width: 768px) {
  .towns-grid {
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  }
  
  .town-image-container {
    height: 160px;
  }
}
    `;
  document.head.appendChild(style);
}

function hideTownsOverlay() {
  const overlay = document.getElementById('townsOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => overlay.remove(), 300);
  }
}

function selectTown(townName) {
  console.log(`Selected town: ${townName}`);
  showAlert(`Selected location: ${townName}`, 'success');
  hideTownsOverlay();
}
              
document.addEventListener('DOMContentLoaded', function() {
    // Select the location link in the menu section
    const menuLocationLink = document.querySelector('.menu-section .location-link');
    
    if (menuLocationLink) {
        menuLocationLink.addEventListener('click', function(e) {
            e.preventDefault(); // Prevent default link behavior
            showTownsOverlay(); // Call the existing function to show towns overlay
        });
    }
});

const locatorsData = [
  // Delhi
  {
    category: "Grocery",
    metropolis: "Delhi",
    details: {
      name: "Dmart",
      image: "https://www.projectsmonitor.com/wp-content/uploads/2021/09/@DMartOfficiall.jpg",
      description: "Your favorite destination for groceries and household essentials at unbeatable prices.",
      address: "Karol Bagh, Delhi",
      timings: "Mon-Sun: 9 AM - 10 PM",
      payment: "Cash, Credit/Debit Cards, UPI",
      mapLink: "https://maps.example.com/dmart",
      menuLink: "https://www.dmart.in/",
      images: [
        "https://www.projectsmonitor.com/wp-content/uploads/2021/09/@DMartOfficiall.jpg",
        "https://www.goodreturns.in/img/1200x60x675/2020/04/dmart-1525851463-1586576977.jpg"
      ],
      social: {
        facebook: "https://facebook.com/dmart",
        twitter: "https://twitter.com/dmart",
        instagram: "https://instagram.com/dmart"
      },
      menu: [
        { name: "Rice", price: "$10/5kg", image: "https://example.com/rice.jpg" },
        { name: "Wheat Flour", price: "$8/5kg", image: "https://example.com/wheat.jpg" }
      ],
      promos: [
        { code: "DMART10", description: "Get 10% off on your first order" }
      ],
      support: {
        email: "support@dmart.com",
        phone: "+91-1234567890"
      },
      outlets: [
        {
          name: "Dmart Karol Bagh",
          address: "Karol Bagh, Delhi",
          timings: "Mon-Sun: 9 AM - 10 PM",
          mapLink: "https://maps.example.com/dmart-karol-bagh"
        },
        {
          name: "Dmart Connaught Place",
          address: "Connaught Place, Delhi",
          timings: "Mon-Sun: 9 AM - 10 PM",
          mapLink: "https://maps.example.com/dmart-connaught-place"
        }
      ]
    }
  },
  {
    category: "Food",
    metropolis: "Delhi",
    details: {
      name: "Burger King",
      image: "https://chawil.com/wp-content/uploads/2022/12/Burger-King-Logo.png",
      description: "Home of the Whopper. Flame-grilled burgers and fries.",
      address: "Connaught Place, Delhi",
      timings: "Mon-Sun: 10 AM - 11 PM",
      payment: "Cash, Credit/Debit Cards, UPI",
      mapLink: "https://maps.example.com/burgerking",
      menuLink: "https://www.burgerking.in/",
      images: [
        "https://example.com/burgerking1.jpg",
        "https://example.com/burgerking2.jpg"
      ],
      social: {
        facebook: "https://facebook.com/burgerking",
        twitter: "https://twitter.com/burgerking",
        instagram: "https://instagram.com/burgerking"
      },
      menu: [
        { name: "Whopper", price: "$5.99", image: "https://example.com/whopper.jpg" },
        { name: "Chicken Fries", price: "$3.99", image: "https://example.com/chicken-fries.jpg" }
      ],
      promos: [
        { code: "BK15", description: "Get 15% off on your first order" }
      ],
      support: {
        email: "support@burgerking.com",
        phone: "+91-9876543210"
      },
      outlets: [
        {
          name: "Burger King Connaught Place",
          address: "Connaught Place, Delhi",
          timings: "Mon-Sun: 10 AM - 11 PM",
          mapLink: "https://maps.example.com/burgerking-cp"
        },
        {
          name: "Burger King Saket",
          address: "Saket, Delhi",
          timings: "Mon-Sun: 10 AM - 11 PM",
          mapLink: "https://maps.example.com/burgerking-saket"
        }
      ]
    }
  },
  {
    category: "Electronics",
    metropolis: "Delhi",
    details: {
      name: "Reliance Digital",
      image: "https://example.com/reliancedigital.jpg",
      description: "Your one-stop shop for electronics and appliances.",
      address: "Nehru Place, Delhi",
      timings: "Mon-Sun: 10 AM - 9 PM",
      payment: "Cash, Credit/Debit Cards, UPI",
      mapLink: "https://maps.example.com/reliancedigital",
      menuLink: "https://www.reliancedigital.in/",
      images: [
        "https://example.com/reliancedigital1.jpg",
        "https://example.com/reliancedigital2.jpg"
      ],
      social: {
        facebook: "https://facebook.com/reliancedigital",
        twitter: "https://twitter.com/reliancedigital",
        instagram: "https://instagram.com/reliancedigital"
      },
      menu: [
        { name: "Smart TV", price: "$500", image: "https://example.com/smarttv.jpg" },
        { name: "Laptop", price: "$800", image: "https://example.com/laptop.jpg" }
      ],
      promos: [
        { code: "RD10", description: "Get 10% off on electronics" }
      ],
      support: {
        email: "support@reliancedigital.com",
        phone: "+91-9876543210"
      },
      outlets: [
        {
          name: "Reliance Digital Nehru Place",
          address: "Nehru Place, Delhi",
          timings: "Mon-Sun: 10 AM - 9 PM",
          mapLink: "https://maps.example.com/reliancedigital-nehru-place"
        },
        {
          name: "Reliance Digital Rajouri Garden",
          address: "Rajouri Garden, Delhi",
          timings: "Mon-Sun: 10 AM - 9 PM",
          mapLink: "https://maps.example.com/reliancedigital-rajouri-garden"
        }
      ]
    }
  },

  // Mumbai
  {
    category: "Grocery",
    metropolis: "Mumbai",
    details: {
      name: "Big Bazaar",
      image: "https://example.com/bigbazaar.jpg",
      description: "India's largest hypermarket chain for groceries and more.",
      address: "Andheri, Mumbai",
      timings: "Mon-Sun: 9 AM - 10 PM",
      payment: "Cash, Credit/Debit Cards, UPI",
      mapLink: "https://maps.example.com/bigbazaar",
      menuLink: "https://www.bigbazaar.com/",
      images: [
        "https://example.com/bigbazaar1.jpg",
        "https://example.com/bigbazaar2.jpg"
      ],
      social: {
        facebook: "https://facebook.com/bigbazaar",
        twitter: "https://twitter.com/bigbazaar",
        instagram: "https://instagram.com/bigbazaar"
      },
      menu: [
        { name: "Rice", price: "$10/5kg", image: "https://example.com/rice.jpg" },
        { name: "Cooking Oil", price: "$15/5L", image: "https://example.com/cooking-oil.jpg" }
      ],
      promos: [
        { code: "BB20", description: "Get 20% off on groceries" }
      ],
      support: {
        email: "support@bigbazaar.com",
        phone: "+91-9876543210"
      },
      outlets: [
        {
          name: "Big Bazaar Andheri",
          address: "Andheri, Mumbai",
          timings: "Mon-Sun: 9 AM - 10 PM",
          mapLink: "https://maps.example.com/bigbazaar-andheri"
        },
        {
          name: "Big Bazaar Borivali",
          address: "Borivali, Mumbai",
          timings: "Mon-Sun: 9 AM - 10 PM",
          mapLink: "https://maps.example.com/bigbazaar-borivali"
        }
      ]
    }
  },
  {
    category: "Food",
    metropolis: "Mumbai",
    details: {
      name: "McDonald's",
      image: "https://wallpapercave.com/wp/wp11260474.png",
      description: "The home of world-famous burgers and fries.",
      address: "Bandra, Mumbai",
      timings: "Mon-Sun: 10 AM - 11 PM",
      payment: "Cash, Credit/Debit Cards, UPI",
      mapLink: "https://maps.example.com/mcdonalds",
      menuLink: "https://mcdelivery.co.in/menu",
      images: [
        "https://example.com/mcd1.jpg",
        "https://example.com/mcd2.jpg"
      ],
      social: {
        facebook: "https://facebook.com/mcdonalds",
        twitter: "https://twitter.com/mcdonalds",
        instagram: "https://instagram.com/mcdonalds"
      },
      menu: [
        { name: "Big Mac", price: "$5.99", image: "https://example.com/bigmac.jpg" },
        { name: "French Fries", price: "$2.99", image: "https://example.com/fries.jpg" }
      ],
      promos: [
        { code: "MCD20", description: "Get 20% off on orders above $20" }
      ],
      support: {
        email: "support@mcdonalds.com",
        phone: "+91-9876543210"
      },
      outlets: [
        {
          name: "McDonald's Bandra",
          address: "Bandra, Mumbai",
          timings: "Mon-Sun: 10 AM - 11 PM",
          mapLink: "https://maps.example.com/mcdonalds-bandra"
        },
        {
          name: "McDonald's Andheri",
          address: "Andheri, Mumbai",
          timings: "Mon-Sun: 10 AM - 11 PM",
          mapLink: "https://maps.example.com/mcdonalds-andheri"
        }
      ]
    }
  },
  {
    category: "Electronics",
    metropolis: "Mumbai",
    details: {
      name: "Vijay Sales",
      image: "https://example.com/vijaysales.jpg",
      description: "Your trusted destination for electronics and appliances.",
      address: "Dadar, Mumbai",
      timings: "Mon-Sun: 10 AM - 9 PM",
      payment: "Cash, Credit/Debit Cards, UPI",
      mapLink: "https://maps.example.com/vijaysales",
      menuLink: "https://www.vijaysales.com/",
      images: [
        "https://example.com/vijaysales1.jpg",
        "https://example.com/vijaysales2.jpg"
      ],
      social: {
        facebook: "https://facebook.com/vijaysales",
        twitter: "https://twitter.com/vijaysales",
        instagram: "https://instagram.com/vijaysales"
      },
      menu: [
        { name: "Smart TV", price: "$500", image: "https://example.com/smarttv.jpg" },
        { name: "Refrigerator", price: "$700", image: "https://example.com/refrigerator.jpg" }
      ],
      promos: [
        { code: "VS15", description: "Get 15% off on electronics" }
      ],
      support: {
        email: "support@vijaysales.com",
        phone: "+91-9876543210"
      },
      outlets: [
        {
          name: "Vijay Sales Dadar",
          address: "Dadar, Mumbai",
          timings: "Mon-Sun: 10 AM - 9 PM",
          mapLink: "https://maps.example.com/vijaysales-dadar"
        },
        {
          name: "Vijay Sales Thane",
          address: "Thane, Mumbai",
          timings: "Mon-Sun: 10 AM - 9 PM",
          mapLink: "https://maps.example.com/vijaysales-thane"
        }
      ]
    }
  },

  // Bangalore
  {
    category: "Grocery",
    metropolis: "Bangalore",
    details: {
      name: "More Megastore",
      image: "https://example.com/moremegastore.jpg",
      description: "A wide range of groceries and household items.",
      address: "Koramangala, Bangalore",
      timings: "Mon-Sun: 9 AM - 10 PM",
      payment: "Cash, Credit/Debit Cards, UPI",
      mapLink: "https://maps.example.com/moremegastore",
      menuLink: "https://www.moremegastore.com/",
      images: [
        "https://example.com/moremegastore1.jpg",
        "https://example.com/moremegastore2.jpg"
      ],
      social: {
        facebook: "https://facebook.com/moremegastore",
        twitter: "https://twitter.com/moremegastore",
        instagram: "https://instagram.com/moremegastore"
      },
      menu: [
        { name: "Rice", price: "$10/5kg", image: "https://example.com/rice.jpg" },
        { name: "Cooking Oil", price: "$15/5L", image: "https://example.com/cooking-oil.jpg" }
      ],
      promos: [
        { code: "MORE10", description: "Get 10% off on groceries" }
      ],
      support: {
        email: "support@moremegastore.com",
        phone: "+91-9876543210"
      },
      outlets: [
        {
          name: "More Megastore Koramangala",
          address: "Koramangala, Bangalore",
          timings: "Mon-Sun: 9 AM - 10 PM",
          mapLink: "https://maps.example.com/moremegastore-koramangala"
        },
        {
          name: "More Megastore Whitefield",
          address: "Whitefield, Bangalore",
          timings: "Mon-Sun: 9 AM - 10 PM",
          mapLink: "https://maps.example.com/moremegastore-whitefield"
        }
      ]
    }
  },
  {
    category: "Food",
    metropolis: "Bangalore",
    details: {
      name: "Domino's Pizza",
      image: "https://example.com/dominos.jpg",
      description: "Hot, fresh pizza delivered to your doorstep.",
      address: "Indiranagar, Bangalore",
      timings: "Mon-Sun: 11 AM - 11 PM",
      payment: "Cash, Credit/Debit Cards, UPI",
      mapLink: "https://maps.example.com/dominos",
      menuLink: "https://www.dominos.co.in/",
      images: [
        "https://example.com/dominos1.jpg",
        "https://example.com/dominos2.jpg"
      ],
      social: {
        facebook: "https://facebook.com/dominos",
        twitter: "https://twitter.com/dominos",
        instagram: "https://instagram.com/dominos"
      },
      menu: [
        { name: "Margherita Pizza", price: "$8", image: "https://example.com/margherita.jpg" },
        { name: "Pepperoni Pizza", price: "$10", image: "https://example.com/pepperoni.jpg" }
      ],
      promos: [
        { code: "DOMINOS20", description: "Get 20% off on your first order" }
      ],
      support: {
        email: "support@dominos.com",
        phone: "+91-9876543210"
      },
      outlets: [
        {
          name: "Domino's Indiranagar",
          address: "Indiranagar, Bangalore",
          timings: "Mon-Sun: 11 AM - 11 PM",
          mapLink: "https://maps.example.com/dominos-indiranagar"
        },
        {
          name: "Domino's Koramangala",
          address: "Koramangala, Bangalore",
          timings: "Mon-Sun: 11 AM - 11 PM",
          mapLink: "https://maps.example.com/dominos-koramangala"
        }
      ]
    }
  },
  {
    category: "Electronics",
    metropolis: "Bangalore",
    details: {
      name: "Croma",
      image: "https://example.com/croma.jpg",
      description: "Your one-stop shop for all electronics and appliances.",
      address: "MG Road, Bangalore",
      timings: "Mon-Sun: 10 AM - 9 PM",
      payment: "Cash, Credit/Debit Cards, UPI",
      mapLink: "https://maps.example.com/croma",
      menuLink: "https://www.croma.com/",
      images: [
        "https://example.com/croma1.jpg",
        "https://example.com/croma2.jpg"
      ],
      social: {
        facebook: "https://facebook.com/croma",
        twitter: "https://twitter.com/croma",
        instagram: "https://instagram.com/croma"
      },
      menu: [
        { name: "Smart TV", price: "$500", image: "https://example.com/smarttv.jpg" },
        { name: "Laptop", price: "$800", image: "https://example.com/laptop.jpg" }
      ],
      promos: [
        { code: "CROMA15", description: "Get 15% off on electronics" }
      ],
      support: {
        email: "support@croma.com",
        phone: "+91-9876543210"
      },
      outlets: [
        {
          name: "Croma MG Road",
          address: "MG Road, Bangalore",
          timings: "Mon-Sun: 10 AM - 9 PM",
          mapLink: "https://maps.example.com/croma-mg-road"
        },
        {
          name: "Croma Whitefield",
          address: "Whitefield, Bangalore",
          timings: "Mon-Sun: 10 AM - 9 PM",
          mapLink: "https://maps.example.com/croma-whitefield"
        }
      ]
    }
  }
];

// State management for overlays
const overlayState = {
  currentOverlay: null,
  navListeners: new Map()
};

// Function to initialize all overlay event listeners
function initializeOverlaySystem() {
  // Clear existing listeners
  overlayState.navListeners.forEach((listener, element) => {
    element.removeEventListener('click', listener);
  });
  overlayState.navListeners.clear();

  // Add listeners for each nav item
  const navItems = {
    'brand': {
      handler: showLocatorsOverlay,
      type: 'locators'
    },
    'services': {
      handler: showProvidersOverlay,
      type: 'providers'
    },
    'places': {
      handler: showPlacesOverlay,
      type: 'places'
    },
    'marketplace': {
      handler: showPlatformsOverlay,
      type: 'platforms'
    }
  };

  Object.entries(navItems).forEach(([page, { handler, type }]) => {
    const navItem = document.querySelector(`.nav-item[data-page="${page}"]`);
    if (navItem) {
      const listener = async (e) => {
        e.preventDefault();
        
        // If there's a current overlay and it's different from the one being opened
        if (overlayState.currentOverlay && overlayState.currentOverlay !== type) {
          // Hide current overlay first
          hideCurrentOverlay();
          
          // Wait for the hide animation to complete
          await new Promise(resolve => setTimeout(resolve, 300));
        }
        
        // Update state and show new overlay
        overlayState.currentOverlay = type;
        handler();
      };
      
      navItem.addEventListener('click', listener);
      overlayState.navListeners.set(navItem, listener);
    }
  });
}

// Debounce function to limit the rate of search execution
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}

// Function to normalize text for better matching
function normalizeText(text) {
  return text.toLowerCase().trim();
}

// Function to check if a string contains words in any order
function containsWordsInAnyOrder(text, searchWords) {
  text = normalizeText(text);
  return searchWords.every(word => text.includes(word));
}

function performSearch(data, searchQuery, gridElement, filterButtons, renderFunction) {
  if (!searchQuery.trim()) {
    // If search query is empty, show all items and activate the 'All' filter button
    gridElement.innerHTML = renderFunction(data);
    filterButtons.forEach(button => {
      if (button.getAttribute('data-category') === 'All') {
        button.classList.add('active');
      } else {
        button.classList.remove('active');
      }
    });
    return;
  }

  const searchWords = normalizeText(searchQuery).split(/\s+/);

  const results = data.filter(item => {
    const searchableText = `
      ${item.category} 
      ${item.metropolis || ''} 
      ${item.details.name} 
      ${item.details.description} 
      ${item.details.address || ''}
      ${item.details.payment || ''}
      ${(item.details.menu || item.details.services || item.details.facilities || []).map(item => item.name).join(' ')}
    `.toLowerCase();

    if (searchableText.includes(searchQuery.toLowerCase())) {
      return true;
    }

    return containsWordsInAnyOrder(searchableText, searchWords);
  });

  const sortedResults = results.sort((a, b) => {
    const aText = `${a.category} ${a.metropolis || ''} ${a.details.name}`.toLowerCase();
    const bText = `${b.category} ${b.metropolis || ''} ${b.details.name}`.toLowerCase();
    
    const aExactMatch = aText.includes(searchQuery.toLowerCase());
    const bExactMatch = bText.includes(searchQuery.toLowerCase());
    
    if (aExactMatch && !bExactMatch) return -1;
    if (!aExactMatch && bExactMatch) return 1;
    
    return 0;
  });

  if (sortedResults.length > 0) {
    gridElement.innerHTML = renderFunction(sortedResults);

    // Determine the most relevant category based on search results
    const firstResultCategory = sortedResults[0].category;
    filterButtons.forEach(button => {
      if (button.getAttribute('data-category') === firstResultCategory) {
        button.classList.add('active');
      } else {
        button.classList.remove('active');
      }
    });
  } else {
    gridElement.innerHTML = `
      <div style="
        grid-column: 1 / -1;
        text-align: center;
        padding: 40px;
        color: #aaa;
        font-size: 1.1em;
      ">
        No results found for "${searchQuery}". Try different keywords or check the spelling.
      </div>`;

    // If no results found, activate the 'All' filter button
    filterButtons.forEach(button => {
      if (button.getAttribute('data-category') === 'All') {
        button.classList.add('active');
      } else {
        button.classList.remove('active');
      }
    });
  }
}

function initializeOverlayVoiceSearch(inputElement, searchFunction) {
  if (!('webkitSpeechRecognition' in window)) {
    const voiceSearchButton = inputElement.nextElementSibling;
    voiceSearchButton.style.display = 'none';
    return;
  }

  const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
  recognition.continuous = false;
  recognition.interimResults = false;
  recognition.lang = 'en-US';

  const voiceSearchButton = inputElement.nextElementSibling;

  voiceSearchButton.addEventListener('click', () => {
    if (voiceSearchButton.classList.contains('active')) {
      recognition.stop();
      voiceSearchButton.classList.remove('active');
    } else {
      recognition.start();
      voiceSearchButton.classList.add('active');
    }
  });

  recognition.onresult = (event) => {
    const transcript = event.results[0][0].transcript;
    inputElement.value = transcript;
    searchFunction(transcript);
    voiceSearchButton.classList.remove('active');
  };

  recognition.onerror = (event) => {
    console.error('Voice recognition error:', event.error);
    voiceSearchButton.classList.remove('active');
    alert('Voice recognition failed. Please try again.');
  };

  recognition.onspeechend = () => {
    recognition.stop();
    voiceSearchButton.classList.remove('active');
  };
}

// Function to show the locators overlay
function showLocatorsOverlay() {
  let overlay = document.getElementById('locatorsOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'locatorsOverlay';
    overlay.className = 'locators-overlay';
    document.body.appendChild(overlay);
  }

  const categories = ['All', ...new Set(locatorsData.map(loc => loc.category))];

  overlay.innerHTML = `
    <div class="locators-content">
      <div class="locators-header">
        <h2>Stores</h2>
        <div class="header-buttons-container">
          <button class="scan-button" onclick="toggleScanner()">
            <i class="fas fa-qrcode"></i>
          </button>
          <button class="close-locators" onclick="hideLocatorsOverlay()">
            <i class="fas fa-long-arrow-alt-left"></i>
          </button>
        </div>
      </div>
      
      <div class="locators-filter-container">
        <div class="locators-filter-buttons">
          ${categories.map(category => `
            <button 
              class="filter-button ${category === 'All' ? 'active' : ''}" 
              data-category="${category}" 
              onclick="filterLocators('${category}')"
            >
              ${category}
            </button>
          `).join('')}
        </div>
      </div>

      <div class="locators-grid" id="locatorsGrid">
        ${renderLocatorCards(locatorsData)}
      </div>
    </div>

    <div class="chat-container">
      <div class="input-container">
        <input type="text" id="locatorsExploreInput" placeholder="Search for services or locations..." />
        <button class="voice-search" id="voiceSearchButton">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  const exploreInput = document.getElementById('locatorsExploreInput');
  const locatorsGrid = document.getElementById('locatorsGrid');
  const filterButtons = document.querySelectorAll('.filter-button');

  const debouncedSearch = debounce((query) => {
    performSearch(locatorsData, query, locatorsGrid, filterButtons, renderLocatorCards);
  }, 300);

  exploreInput.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
  });

  exploreInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
      performSearch(locatorsData, exploreInput.value, locatorsGrid, filterButtons, renderLocatorCards);
    }
  });

  initializeOverlayVoiceSearch(exploreInput, (query) => {
    performSearch(locatorsData, query, locatorsGrid, filterButtons, renderLocatorCards);
  });

  setTimeout(() => overlay.classList.add('active'), 10);


  const style = document.createElement('style');
  style.textContent = `
  .locators-overlay {
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  bottom: 60px;
  background: #1a1a1f;
  z-index: 1000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  overflow-y: auto;
  width: 100%;
  display: flex;
  flex-direction: column;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.locators-overlay::-webkit-scrollbar {
  display: none;
}

.locators-overlay.active {
  opacity: 1;
  visibility: visible;
}

.locators-content {
  flex: 1;
  width: 100%;
  max-width: 100%;
  padding: 20px;
  margin: 0 auto;
  position: relative;
}

.locators-header {
  padding: 16px 24px;
  background: rgba(26, 26, 31, 0.95);
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 30;
  border-bottom: 1px solid rgba(255, 215, 0, 0.15);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  height: 72px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
}

.locators-header h2 {
  color: var(--primary-color);
  margin: 0;
  font-size: 1.75em;
  font-weight: 600;
  letter-spacing: -0.02em;
  display: flex;
  align-items: center;
  gap: 12px;
}

.locators-header h2::before {
  content: '';
  display: inline-block;
  width: 24px;
  height: 24px;
  background: var(--primary-color);
  mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpath d='M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z'%3E%3C/path%3E%3Ccircle cx='12' cy='10' r='3'%3E%3C/circle%3E%3C/svg%3E");
  mask-repeat: no-repeat;
  mask-position: center;
  mask-size: contain;
}

.header-buttons-container {
  display: flex;
  align-items: center;
  gap: 16px;
}

.close-locators {
  background: rgba(255, 215, 0, 0.1);
  border: 1px solid rgba(255, 215, 0, 0.2);
  color: var(--primary-color);
  font-size: 1.25em;
  cursor: pointer;
  padding: 8px 16px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.close-locators:hover {
  background: rgba(255, 215, 0, 0.15);
  transform: translateY(-1px);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.close-locators:active {
  transform: translateY(0);
}

    .scan-button {
      background: rgba(255, 215, 0, 0.1);
      border: 1px solid rgba(255, 215, 0, 0.2);
      color: var(--primary-color);
      font-size: 1.25em;
      cursor: pointer;
      padding: 8px 16px;
      border-radius: 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: all 0.2s ease;
    }

    .scan-button:hover {
      background: rgba(255, 215, 0, 0.15);
      transform: translateY(-1px);
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }

    .scan-button:active {
      transform: translateY(0);
    }

    .scan-button i {
      font-size: 1.1em;
    }

    /* Mobile responsiveness */
    @media (max-width: 768px) {
      .header-buttons-container {
        gap: 8px;
      }

      .scan-button {
        padding: 8px 12px;
        font-size: 1.1em;
      }
    }

.locators-filter-container {
  position: fixed;
  top: 70px;
  left: 0;
  width: 100vw;
  max-width: 100vw;
  z-index: 10;
  background: #1a1a1f;
  margin-left: 10px;
  padding: 10px 15px;
  box-sizing: border-box;
}

.locators-filter-buttons {
  display: flex;
  justify-content: flex-start;
  gap: 4px;
  padding-bottom: 10px;
  width: 100vw;
  margin-left: calc(-50vw + 50%);
  overflow-x: auto;
  scrollbar-width: none;
}

.locators-filter-buttons::-webkit-scrollbar {
  height: 0px;
}

.locators-filter-buttons::-webkit-scrollbar-thumb {
  background-color: var(--primary-color);
  border-radius: 3px;
}

.filter-button {
  margin-top: 2px;
  margin-left: 2px;
  margin-right: 5px;
  flex-shrink: 0;
  background: rgba(255, 215, 0, 0.1);
  color: var(--text-light);
  border: 1px solid rgba(255, 215, 0, 0.2);
  padding: 10px 15px;
  border-radius: 25px;
  transition: all 0.3s ease;
  cursor: pointer;
  white-space: nowrap;
  font-weight: 500;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.filter-button:hover {
  background: var(--primary-color);
  color: black;
  transform: scale(1.05);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.filter-button.active {
  background: var(--primary-color);
  color: black;
  font-weight: 700;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
}

.locators-grid-container {
  flex: 1;
  width: 100%;
  padding-bottom: 20px;
  position: relative;
}

.locators-grid {
  margin-top: 70px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
  gap: 20px;
  animation: fadeInUp 0.5s ease-out;
  padding-bottom: 140px;
  width: 100%;
}

.locator-card {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 16px;
  overflow: hidden;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  cursor: pointer;
}

.locator-card:hover {
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  border-color: rgba(255, 215, 0, 0.3);
}

.locator-card-header {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 20px;
  background: rgba(255, 255, 255, 0.03);
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
}

.brand-logo {
  width: 60px;
  height: 60px;
  border-radius: 12px;
  object-fit: cover;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.locator-card-title {
  flex: 1;
}

.locator-card-title h3 {
  margin: 0;
  font-size: 1.2em;
  color: #fff;
  font-weight: 600;
}

.category-tag {
  margin: 5px 0 0;
  font-size: 0.9em;
  color: rgba(255, 215, 0, 0.8);
  font-weight: 500;
}

.locator-card-body {
  padding: 20px;
}

.locator-description {
  margin: 0;
  font-size: 0.95em;
  color: rgba(255, 255, 255, 0.8);
  line-height: 1.5;
}

.locator-card-actions {
  display: flex;
  gap: 10px;
  padding: 15px 20px;
  background: rgba(255, 255, 255, 0.03);
  border-top: 1px solid rgba(255, 255, 255, 0.05);
}

.action-button {
  flex: 1;
  background: rgba(255, 215, 0, 0.1);
  border: none;
  border-radius: 8px;
  padding: 10px;
  color: #fff;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-size: 0.9em;
}

.action-button:hover {
  background: rgba(255, 215, 0, 0.2);
  transform: translateY(-2px);
}

.action-button i {
  font-size: 1.1em;
}

/* Responsive Design */
@media (max-width: 768px) {
  .locator-card-header {
    padding: 15px;
  }

  .brand-logo {
    width: 50px;
    height: 50px;
  }

  .locator-card-title h3 {
    font-size: 1.1em;
  }

  .locator-card-body {
    padding: 15px;
  }

  .locator-card-actions {
    padding: 10px 15px;
    flex-wrap: wrap;
  }

  .action-button {
    flex: 1 1 45%;
    padding: 8px;
    font-size: 0.85em;
  }
}

  .chat-container {
    position: fixed;
    bottom: 60px;
    left: 0;
    right: 0;
    width: 100%;
    max-width: 100%;
    z-index: 1000;
    padding: 20px;
    background: #1a1a1f;
    border-top: 1px solid rgba(255, 255, 255, 0.1);
    display: flex;
    gap: 12px;
    align-items: center;
    box-sizing: border-box;
  }

  .input-container {
    flex: 1;
    position: relative;
    display: flex;
    align-items: center;
  }

  #locatorsExploreInput {
    flex: 1;
    background: rgba(255, 255, 255, 0.05);
    border: 1px solid rgba(255, 215, 0, 0.2);
    border-radius: 12px;
    padding: 12px 50px 12px 20px;
    color: #fff;
    font-size: 0.95em;
    width: 100%;
    transition: border-color 0.3s ease;
  }

  #locatorsExploreInput:focus {
    border-color: rgba(255, 255, 255, 0.5);
  }

  .voice-search {
    position: absolute;
    right: 10px;
    background: none;
    border: none;
    color: var(--primary-color);
    cursor: pointer;
    padding: 10px;
    transition: all 0.3s ease;
}

  .voice-search:hover {
    opacity: 0.8;
  }

  .voice-search.active {
    color: #ffd700;
    animation: pulse 1.5s infinite;
  }

  .voice-search i { 
    font-size: 1.5em; 
}

  @keyframes pulse {
    0% { transform: scale(1); }
    50% { transform: scale(1.1); }
    100% { transform: scale(1); }
  }

/* Desktop Specific Styles */
@media (min-width: 769px) {
  .chat-container {
    padding: 24px;
    bottom: 60px;
  }

  .input-container {
    max-width: 800px;
    margin: 0 auto;
  }

  #locatorsExploreInput {
    height: 80px;
    font-size: 1.05rem;
    padding: 0 50px 0 24px;
    border-radius: 16px;
  }

  .voice-search {
    width: 50px;
    height: 50px;
    right: 8px;
  }

  .voice-search i {
    font-size: 2.1em;
  }
}

/* Tablet Specific Styles */
@media (min-width: 769px) and (max-width: 1024px) {
  .input-container {
    max-width: 700px;
  }
}

/* Desktop and laptop styles */
@media (min-width: 769px) {
  .chat-container {
    width: 100%;
    display: flex;
    justify-content: center;
  }
  
  #locatorsExploreInput {
    width: 60%;
    max-width: 1200px;
  }
  
  .locators-grid {
    padding-bottom: 180px;
  }
}

/* Mobile styles */
@media (max-width: 768px) {
  .chat-container {
    width: 100%;
    left: 0;
    transform: none;
    padding: 10px;
    gap: 8px;
  }

  .locators-grid {
    grid-template-columns: 1fr;
    padding: 0 10px;
    margin-top: 80px;
    gap: 15px;
  }

  .locator-card {
    width: 100%;
    margin: 0;
  }

  .locators-grid-container {
    width: 100%;
    max-width: 100%;
    padding: 0;
  }

  .locators-content {
    padding: 10px 0;
    padding-bottom: 200px;
  }

  #locatorsExploreInput {
    width: 100%;
    padding: 20px 20px;
  }
}
   `;
  document.head.appendChild(style);
}

// Function to close details page
function closeLocatorDetails() {
  const detailsPage = document.querySelector('.locator-details-page');
  if (detailsPage) {
    detailsPage.style.transform = 'translateX(100%)';
    setTimeout(() => detailsPage.remove(), 300);
  }
}

// Carousel functions
let currentSlideIndex = 1;

function changeSlide(n) {
  showSlides(currentSlideIndex += n);
}

function currentSlide(n) {
  showSlides(currentSlideIndex = n);
}

function showSlides(n) {
  const dots = document.getElementsByClassName("dot");
  if (n > dots.length) currentSlideIndex = 1;
  if (n < 1) currentSlideIndex = dots.length;

  // Update dots
  for (let i = 0; i < dots.length; i++) {
    dots[i].className = dots[i].className.replace(" active", "");
  }
  dots[currentSlideIndex-1].className += " active";
}

// Share function
function shareLocation(name) {
  if (navigator.share) {
    navigator.share({
      title: name,
      text: `Check out ${name} on our platform!`,
      url: window.location.href
    }).catch(console.error);
  } else {
    // Fallback
    const dummy = document.createElement('input');
    document.body.appendChild(dummy);
    dummy.value = window.location.href;
    dummy.select();
    document.execCommand('copy');
    document.body.removeChild(dummy);
    
    alert('Link copied to clipboard!');
  }
}

function renderLocatorCards(filteredData) {
  return filteredData.map(locator => {
    // Determine the button text based on the category
    let buttonText = 'Menu'; // Default to 'Menu'
    if (locator.category === 'Electronics') {
      buttonText = 'Products';
    } else if (locator.category === 'Grocery') {
      buttonText = 'Products';
    } else if (locator.category === 'Food') {
      buttonText = 'Menu';
    }

    return `
      <div class="locator-card" onclick="openStorePage('${locator.category}')">
        <div class="locator-card-header">
          <img src="${locator.details.image}" alt="${locator.details.name}" class="brand-logo">
          <div class="locator-card-title">
            <h3>${locator.details.name}</h3>
            <p class="category-tag">${locator.category}</p>
          </div>
        </div>
        <div class="locator-card-body">
          <p class="locator-description">${locator.details.description}</p>
        </div>
        <div class="locator-card-actions">
          ${locator.details.menu ? `
            <a href="${locator.details.menuLink}" target="_blank" class="action-button" onclick="event.stopPropagation()">
              <i class="fas fa-utensils"></i> ${buttonText}
            </a>
          ` : ''}
          <a href="${locator.details.mapLink}" target="_blank" class="action-button" onclick="event.stopPropagation()">
            <i class="fas fa-map-marker-alt"></i> Map
          </a>
          <button class="action-button" onclick="openStorePage('${locator.category}'); event.stopPropagation()">
            <i class="fas fa-info-circle"></i> Info
          </button>
          <!-- Chat Button for Dmart -->
          <button class="action-button chat-button" onclick="showChatSupport('${locator.details.name}'); event.stopPropagation()">
            <i class="fas fa-comments"></i> Chat
          </button>
        </div>
      </div>
    `;
  }).join('');
}

function openStorePage(category) {
  const selectedMetropolis = localStorage.getItem('selectedMetropolis') || 'Select City';
  const locator = locatorsData.find(loc => loc.category === category);
  if (!locator) return;

  // Filter outlets based on the selected metropolis
  const filteredOutlets = locator.details.outlets.filter(outlet => {
    return outlet.address.includes(selectedMetropolis);
  });

  const storePage = document.createElement('div');
  storePage.className = 'store-page';
  storePage.innerHTML = `
    <div class="store-header">
      <button class="back-button" onclick="closeStorePage()">
        <i class="fas fa-arrow-left"></i> Back
      </button>
      <h2>${locator.details.name}</h2>
    </div>
    <div class="store-content">
      <!-- Store Info -->
      <div class="store-info">
        <img src="${locator.details.image}" alt="${locator.details.name}" class="store-logo">
        <h3>${locator.details.name}</h3>
        <p class="store-description">${locator.details.description}</p>
        <div class="social-media">
          <a href="${locator.details.social?.facebook}" target="_blank"><i class="fab fa-facebook"></i></a>
          <a href="${locator.details.social?.twitter}" target="_blank"><i class="fab fa-twitter"></i></a>
          <a href="${locator.details.social?.instagram}" target="_blank"><i class="fab fa-instagram"></i></a>
        </div>
      </div>
      
      <!-- Tabs -->
      <div class="store-tabs">
        <button class="tab-button active" data-tab="locator">Store Locator</button>
        <button class="tab-button" data-tab="catalog">Product Catalog</button>
        <button class="tab-button" data-tab="promos">Promos & Deals</button>
        <button class="tab-button" data-tab="support">Customer Support</button>
      </div>
      
      <!-- Tab Content -->
      <div class="tab-content">
        <div id="locator" class="tab-pane active">
          <!-- Store Locator Content -->
          <div class="store-locator">
            <h4>Find Stores in ${selectedMetropolis}</h4>
            <div class="locator-grid" id="locatorGrid">
              ${renderOutlets(filteredOutlets)}
            </div>
          </div>
        </div>
        <div id="catalog" class="tab-pane">
          <!-- Product Catalog Content -->
          <div class="product-catalog">
            <h4>Available Products</h4>
            <div class="product-grid" id="productGrid">
              ${locator.details.menu?.map(item => `
                <div class="product-item">
                  <img src="${item.image}" alt="${item.name}">
                  <h5>${item.name}</h5>
                  <p>${item.price}</p>
                  <button class="add-to-cart" onclick="addToCart('${item.name}', '${item.price}')">
                    <i class="fas fa-cart-plus"></i> Add to Cart
                  </button>
                </div>
              `).join('')}
            </div>
          </div>
        </div>
        <div id="promos" class="tab-pane">
          <!-- Promos & Deals Content -->
          <div class="promos-deals" id="promosGrid">
            <h4>Available Promos & Deals</h4>
            ${locator.details.promos?.map(promo => `
              <div class="promo-item">
                <p>${promo.code} - ${promo.description}</p>
                <button onclick="copyPromo('${promo.code}')">Copy Code</button>
              </div>
            `).join('')}
          </div>
        </div>
        <div id="support" class="tab-pane">
          <!-- Customer Support Content -->
          <div class="customer-support" id="supportContent">
            <h4>Contact Support</h4>
            <p>Email: ${locator.details.support?.email}</p>
            <p>Phone: ${locator.details.support?.phone}</p>
            <button onclick="showChatSupport('${locator.category}')">
              <i class="fas fa-comments"></i> Chat with Us
            </button>
          </div>
        </div>
      </div>
      
      <!-- Search Container -->
      <div class="chat-container">
        <input type="text" id="storeInput" placeholder="Search for services, products, or locations..." />
        <div class="input-actions">
          <button class="send-message" id="storeButton">
            <i class="fas fa-paper-plane"></i>
          </button>
        </div>
      </div>
    </div>
  `;

  document.body.appendChild(storePage);
  addStorePageStyles();
  initializeTabs();
  initializeSearch();
  scrollToTop();
}

function renderOutlets(outlets) {
  return outlets.map(outlet => `
    <div class="outlet-card">
      <h5>${outlet.name}</h5>
      <p>${outlet.address}</p>
      <p>${outlet.timings}</p>
      <a href="${outlet.mapLink}" target="_blank" class="action-button">
        <i class="fas fa-map-marker-alt"></i> View on Map
      </a>
    </div>
  `).join('');
}

// Function to scroll to the top of the store page
function scrollToTop() {
  const storeContent = document.querySelector('.store-content');
  if (storeContent) {
    storeContent.scrollTo({ top: 0, behavior: 'smooth' });
  }
}

// Function to initialize search functionality with debounce
function initializeSearch() {
  const storeInput = document.getElementById('storeInput');
  if (storeInput) {
    storeInput.addEventListener('input', debounce((e) => {
      const searchTerm = e.target.value.toLowerCase().trim();
      const intent = understandUserIntent(searchTerm);
      filterStoreContent(searchTerm, intent);
    }, 300));
  }
}

// Function to understand user intent using simple NLP
function understandUserIntent(searchTerm) {
  if (searchTerm.includes('offer') || searchTerm.includes('deal')) {
    return 'promos';
  } else if (searchTerm.includes('product') || searchTerm.includes('item')) {
    return 'catalog';
  } else if (searchTerm.includes('support') || searchTerm.includes('contact')) {
    return 'support';
  } else if (searchTerm.includes('store') || searchTerm.includes('location')) {
    return 'locator';
  }
  return null;
}

// Function to filter store content based on search term and intent
function filterStoreContent(searchTerm, intent) {
  const storeContent = document.querySelector('.store-content');
  const tabPanes = document.querySelectorAll('.tab-pane');
  const noResultsMessage = document.getElementById('noResultsMessage');

  if (!searchTerm) {
    // If search term is empty, show all content
    tabPanes.forEach(pane => pane.style.display = 'block');
    if (noResultsMessage) noResultsMessage.remove();
    return;
  }

  // Hide all tab panes initially
  tabPanes.forEach(pane => pane.style.display = 'none');

  // Switch to the relevant tab based on intent
  if (intent) {
    document.querySelector(`[data-tab="${intent}"]`).click();
  }

  // Filter Product Catalog
  const productGrid = document.getElementById('productGrid');
  if (productGrid) {
    const products = productGrid.querySelectorAll('.product-item');
    let productMatch = false;
    products.forEach(product => {
      const productName = product.querySelector('h5').textContent.toLowerCase();
      const productDescription = product.querySelector('p').textContent.toLowerCase();
      if (productName.includes(searchTerm) || productDescription.includes(searchTerm)) {
        product.style.display = 'block';
        productMatch = true;
      } else {
        product.style.display = 'none';
      }
    });
    if (productMatch) {
      document.getElementById('catalog').style.display = 'block';
    }
  }

  // Filter Promos & Deals
  const promosGrid = document.getElementById('promosGrid');
  if (promosGrid) {
    const promos = promosGrid.querySelectorAll('.promo-item');
    let promoMatch = false;
    promos.forEach(promo => {
      const promoText = promo.textContent.toLowerCase();
      if (promoText.includes(searchTerm)) {
        promo.style.display = 'flex';
        promoMatch = true;
      } else {
        promo.style.display = 'none';
      }
    });
    if (promoMatch) {
      document.getElementById('promos').style.display = 'block';
    }
  }

  // Filter Customer Support
  const supportContent = document.getElementById('supportContent');
  if (supportContent) {
    const supportText = supportContent.textContent.toLowerCase();
    if (supportText.includes(searchTerm)) {
      document.getElementById('support').style.display = 'block';
    }
  }

  // Filter Store Locator
  const locatorGrid = document.getElementById('locatorGrid');
  if (locatorGrid) {
    const locators = locatorGrid.querySelectorAll('.locator-card');
    let locatorMatch = false;
    locators.forEach(locator => {
      const locatorText = locator.textContent.toLowerCase();
      if (locatorText.includes(searchTerm)) {
        locator.style.display = 'block';
        locatorMatch = true;
      } else {
        locator.style.display = 'none';
      }
    });
    if (locatorMatch) {
      document.getElementById('locator').style.display = 'block';
    }
  }

  // Show no results message if no matches found
  if (!productMatch && !promoMatch && !supportText?.includes(searchTerm) && !locatorMatch) {
    if (!noResultsMessage) {
      const noResultsDiv = document.createElement('div');
      noResultsDiv.id = 'noResultsMessage';
      noResultsDiv.textContent = 'No results found.';
      noResultsDiv.style.textAlign = 'center';
      noResultsDiv.style.color = 'rgba(255, 255, 255, 0.8)';
      noResultsDiv.style.marginTop = '20px';
      storeContent.appendChild(noResultsDiv);
    }
  } else {
    if (noResultsMessage) noResultsMessage.remove();
  }

  // Scroll to the top of the store page
  scrollToTop();
}

// Function to close the store page
function closeStorePage() {
  const storePage = document.querySelector('.store-page');
  if (storePage) {
    storePage.remove();
  }
}

// Function to initialize tabs on the store page
function initializeTabs() {
  const tabButtons = document.querySelectorAll('.tab-button');
  const tabPanes = document.querySelectorAll('.tab-pane');

  tabButtons.forEach(button => {
    button.addEventListener('click', () => {
      const tabName = button.getAttribute('data-tab');
      tabButtons.forEach(btn => btn.classList.remove('active'));
      tabPanes.forEach(pane => pane.classList.remove('active'));
      button.classList.add('active');
      document.getElementById(tabName).classList.add('active');
    });
  });
}

function addStorePageStyles() {
  const style = document.createElement('style');
  style.textContent = `
    /* Store Page Container */
    .store-page {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: #1a1a1f;
      z-index: 2000;
      overflow-y: auto;
      padding: 20px;
      box-sizing: border-box;
      font-family: 'Inter', sans-serif;
    }

    /* Store Header */
    .store-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 20px;
      padding: 15px;
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      backdrop-filter: blur(10px);
    }

    .back-button {
      background: none;
      border: none;
      color: #fff;
      font-size: 1.5em;
      cursor: pointer;
      transition: transform 0.2s ease, opacity 0.2s ease;
    }

    .back-button:hover {
      transform: scale(1.1);
      opacity: 0.8;
    }

    .store-header h2 {
      margin: 0;
      font-size: 1.5em;
      color: #fff;
      font-weight: 600;
      letter-spacing: -0.5px;
    }

    /* Store Info */
    .store-info {
      text-align: center;
      margin-bottom: 20px;
    }

    .store-logo {
      width: 100px;
      height: 100px;
      border-radius: 50%;
      object-fit: cover;
      border: 3px solid rgba(255, 215, 0, 0.5);
      margin-bottom: 15px;
      transition: transform 0.3s ease;
    }

    .store-logo:hover {
      transform: scale(1.05);
    }

    .store-info h3 {
      margin: 0;
      font-size: 1.75em;
      color: #fff;
      font-weight: 600;
      letter-spacing: -0.5px;
    }

    .store-description {
      color: rgba(255, 255, 255, 0.8);
      font-size: 1em;
      margin-top: 10px;
      line-height: 1.5;
    }

    /* Social Media Icons */
    .social-media {
      display: flex;
      justify-content: center;
      gap: 20px;
      margin-top: 20px;
    }

    .social-media a {
      color: #fff;
      font-size: 1.75em;
      transition: transform 0.3s ease, color 0.3s ease;
    }

    .social-media a:hover {
      color: #ffd700;
      transform: scale(1.2);
    }

    /* Tabs */
    .store-tabs {
      display: flex;
      justify-content: space-around;
      margin-bottom: 20px;
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      padding: 10px;
      backdrop-filter: blur(10px);
    }

    .tab-button {
      background: none;
      border: none;
      color: rgba(255, 255, 255, 0.7);
      font-size: 1em;
      cursor: pointer;
      padding: 12px 24px;
      border-radius: 12px;
      transition: all 0.3s ease;
    }

    .tab-button.active {
      background: rgba(255, 215, 0, 0.1);
      color: #ffd700;
      font-weight: 600;
    }

    .tab-button:hover {
      background: rgba(255, 215, 0, 0.1);
    }

    /* Tab Content */
    .tab-content {
      padding-bottom: 200px;
    }

    .tab-pane {
      display: none;
      animation: fadeIn 0.5s ease;
    }

    .tab-pane.active {
      display: block;
    }

    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }

    /* Product Catalog */
    .product-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
      gap: 20px;
    }

    .product-item {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      padding: 20px;
      text-align: center;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      backdrop-filter: blur(10px);
    }

    .product-item:hover {
      transform: translateY(-5px);
      box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
    }

    .product-item img {
      width: 100%;
      height: auto;
      border-radius: 12px;
      margin-bottom: 15px;
      transition: transform 0.3s ease;
    }

    .product-item:hover img {
      transform: scale(1.05);
    }

    .product-item h5 {
      margin: 0;
      font-size: 1.1em;
      color: #fff;
      font-weight: 600;
    }

    .product-item p {
      margin: 10px 0;
      color: rgba(255, 255, 255, 0.8);
      font-size: 0.95em;
    }

    .add-to-cart {
      background: rgba(255, 215, 0, 0.1);
      border: none;
      color: #fff;
      padding: 10px 20px;
      border-radius: 12px;
      cursor: pointer;
      transition: background 0.3s ease, transform 0.3s ease;
    }

    .add-to-cart:hover {
      background: rgba(255, 215, 0, 0.2);
      transform: scale(1.05);
    }

    /* Promos & Deals */
    .promos-deals {
      display: grid;
      gap: 20px;
    }

    .promo-item {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      padding: 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      backdrop-filter: blur(10px);
    }

    .promo-item p {
      margin: 0;
      color: rgba(255, 255, 255, 0.8);
      font-size: 1em;
    }

    .promo-item button {
      background: rgba(255, 215, 0, 0.1);
      border: none;
      color: #fff;
      padding: 10px 20px;
      border-radius: 12px;
      cursor: pointer;
      transition: background 0.3s ease, transform 0.3s ease;
    }

    .promo-item button:hover {
      background: rgba(255, 215, 0, 0.2);
      transform: scale(1.05);
    }

    /* Customer Support */
    .customer-support {
      text-align: center;
      padding: 20px;
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      backdrop-filter: blur(10px);
    }

    .customer-support p {
      margin: 10px 0;
      color: rgba(255, 255, 255, 0.8);
      font-size: 1em;
    }

    .customer-support button {
      background: rgba(255, 215, 0, 0.1);
      border: none;
      color: #fff;
      padding: 12px 24px;
      border-radius: 12px;
      cursor: pointer;
      transition: background 0.3s ease, transform 0.3s ease;
    }

    .customer-support button:hover {
      background: rgba(255, 215, 0, 0.2);
      transform: scale(1.05);
    }

    /* Chat Container */
    .chat-container {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      width: 100%;
      max-width: 100%;
      z-index: 1000;
      padding: 20px;
      background: #1a1a1f;
      border-top: 1px solid rgba(255, 215, 0, 0.1);
      display: flex;
      gap: 12px;
      align-items: center;
      box-sizing: border-box;
      backdrop-filter: blur(10px);
    }

    #storeInput {
      flex: 1;
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid rgba(255, 215, 0, 0.2);
      border-radius: 12px;
      padding: 20px;
      color: #fff;
      font-size: 1em;
      width: 100%;
      transition: border-color 0.3s ease;
    }

    #storeInput:focus {
      border-color: rgba(255, 215, 0, 0.5);
    }

        /* Outlet Card Styles */
    .outlet-card {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      padding: 20px;
      margin-bottom: 20px;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      backdrop-filter: blur(10px);
    }

    .outlet-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
    }

    .outlet-card h5 {
      margin: 0;
      font-size: 1.1em;
      color: #fff;
      font-weight: 600;
    }

    .outlet-card p {
      margin: 10px 0;
      color: rgba(255, 255, 255, 0.8);
      font-size: 0.95em;
    }

    .outlet-card .action-button {
      background: rgba(255, 215, 0, 0.1);
      border: none;
      color: #fff;
      padding: 10px 20px;
      border-radius: 12px;
      cursor: pointer;
      transition: background 0.3s ease, transform 0.3s ease;
      display: inline-block;
      margin-top: 10px;
    }

    .outlet-card .action-button:hover {
      background: rgba(255, 215, 0, 0.2);
      transform: scale(1.05);
    }

    /* Responsive Design */
    @media (max-width: 768px) {
      .store-header h2 {
        font-size: 1.2em;
      }

      .store-logo {
        width: 80px;
        height: 80px;
      }

      .store-description {
        font-size: 0.9em;
      }

      .tab-button {
        font-size: 0.9em;
        padding: 10px 20px;
      }

      .product-grid {
        grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
      }

      .product-item h5 {
        font-size: 1em;
      }

      .product-item p {
        font-size: 0.85em;
      }

      .add-to-cart {
        font-size: 0.85em;
      }

      .promo-item p {
        font-size: 0.9em;
      }

      .promo-item button {
        font-size: 0.85em;
      }

      .customer-support button {
        font-size: 0.9em;
      }
    }

    /* Small Mobile Optimizations */
    @media (max-width: 480px) {
      .store-page {
        padding: 10px;
      }

      .product-grid {
        grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
        gap: 10px;
      }

      .tab-button {
        padding: 8px 16px;
        font-size: 0.85em;
      }

      .chat-container {
        padding: 20px;
      }

      #storeInput {
        padding: 20px;
      }
    }
  `;
  document.head.appendChild(style);
}

// Helper function to escape HTML special characters
function escapeHTML(str) {
  const div = document.createElement('div');
  div.textContent = str;
  return div.innerHTML;
}

// Function to filter locators based on category
function filterLocators(category) {
  const locatorsGrid = document.getElementById('locatorsGrid');
  let filteredData = locatorsData;

  if (category !== 'All') {
    filteredData = filteredData.filter(locator => locator.category === category);
  }

  locatorsGrid.innerHTML = renderLocatorCards(filteredData);

  const filterButtons = document.querySelectorAll('.filter-button');
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === category) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

// Additional placeholder functions (you might want to implement these)
function selectLocator(category) {
  console.log(`Selected locator in category: ${category}`);
}

function selectLocator(category) {
  // Optional: Add any specific action when a locator card is clicked
  console.log(`Selected locator in category: ${category}`);
}

const serviceProvidersData = [
  // Delhi
  {
    category: "Plumber",
    metropolis: "Delhi",
    details: {
      name: "Delhi Plumbing Services",
      image: "https://example.com/plumber.jpg",
      description: "Professional plumbing services for homes and businesses.",
      address: "Karol Bagh, Delhi",
      timings: "Mon-Sun: 8 AM - 8 PM",
      payment: "Cash, Credit/Debit Cards, UPI",
      mapLink: "https://maps.example.com/plumber",
      serviceLink: "https://www.delhiplumbing.com/",
      images: [
        "https://example.com/plumber1.jpg",
        "https://example.com/plumber2.jpg"
      ],
      social: {
        facebook: "https://facebook.com/delhiplumbing",
        twitter: "https://twitter.com/delhiplumbing",
        instagram: "https://instagram.com/delhiplumbing"
      },
      services: [
        { name: "Pipe Repair", price: "$50", image: "https://example.com/pipe-repair.jpg" },
        { name: "Leak Fixing", price: "$30", image: "https://example.com/leak-fixing.jpg" }
      ],
      promos: [
        { code: "PLUMB10", description: "Get 10% off on your first service" }
      ],
      support: {
        email: "support@delhiplumbing.com",
        phone: "+91-1234567890"
      },
      outlets: [
        {
          name: "Delhi Plumbing Karol Bagh",
          address: "Karol Bagh, Delhi",
          timings: "Mon-Sun: 8 AM - 8 PM",
          mapLink: "https://maps.example.com/plumber-karol-bagh"
        },
        {
          name: "Delhi Plumbing Connaught Place",
          address: "Connaught Place, Delhi",
          timings: "Mon-Sun: 8 AM - 8 PM",
          mapLink: "https://maps.example.com/plumber-connaught-place"
        }
      ]
    }
  },
  {
    category: "Electrician",
    metropolis: "Delhi",
    details: {
      name: "Delhi Electric Services",
      image: "https://example.com/electrician.jpg",
      description: "Certified electricians for all your electrical needs.",
      address: "Connaught Place, Delhi",
      timings: "Mon-Sun: 9 AM - 9 PM",
      payment: "Cash, Credit/Debit Cards, UPI",
      mapLink: "https://maps.example.com/electrician",
      serviceLink: "https://www.delhielectric.com/",
      images: [
        "https://example.com/electrician1.jpg",
        "https://example.com/electrician2.jpg"
      ],
      social: {
        facebook: "https://facebook.com/delhielectric",
        twitter: "https://twitter.com/delhielectric",
        instagram: "https://instagram.com/delhielectric"
      },
      services: [
        { name: "Wiring", price: "$100", image: "https://example.com/wiring.jpg" },
        { name: "Fuse Box Repair", price: "$60", image: "https://example.com/fuse-box.jpg" }
      ],
      promos: [
        { code: "ELEC15", description: "Get 15% off on your first service" }
      ],
      support: {
        email: "support@delhielectric.com",
        phone: "+91-9876543210"
      },
      outlets: [
        {
          name: "Delhi Electric Connaught Place",
          address: "Connaught Place, Delhi",
          timings: "Mon-Sun: 9 AM - 9 PM",
          mapLink: "https://maps.example.com/electrician-cp"
        },
        {
          name: "Delhi Electric Saket",
          address: "Saket, Delhi",
          timings: "Mon-Sun: 9 AM - 9 PM",
          mapLink: "https://maps.example.com/electrician-saket"
        }
      ]
    }
  },
  // Add more service providers as needed
];

// Function to show the providers overlay
function showProvidersOverlay() {
  let overlay = document.getElementById('providersOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'providersOverlay';
    overlay.className = 'providers-overlay';
    document.body.appendChild(overlay);
  }

  const categories = ['All', ...new Set(serviceProvidersData.map(provider => provider.category))];

  overlay.innerHTML = `
    <div class="providers-content">
      <div class="providers-header">
        <h2>Service Providers</h2>
        <button class="close-providers" onclick="hideProvidersOverlay()">
          <i class="fas fa-long-arrow-alt-left"></i>
        </button>
      </div>
      
      <div class="providers-filter-container">
        <div class="providers-filter-buttons">
          ${categories.map(category => `
            <button 
              class="filter-button ${category === 'All' ? 'active' : ''}" 
              data-category="${category}" 
              onclick="filterProviders('${category}')"
            >
              ${category}
            </button>
          `).join('')}
        </div>
      </div>

      <div class="providers-grid" id="providersGrid">
        ${renderProviderCards(serviceProvidersData)}
      </div>
    </div>

    <div class="chat-container">
      <div class="input-container">
        <input type="text" id="providersExploreInput" placeholder="Search for services or locations..." />
        <button class="voice-search" id="voiceSearchButton">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  const exploreInput = document.getElementById('providersExploreInput');
  const providersGrid = document.getElementById('providersGrid');
  const filterButtons = document.querySelectorAll('.filter-button');

  const debouncedSearch = debounce((query) => {
    performSearch(serviceProvidersData, query, providersGrid, filterButtons, renderProviderCards);
  }, 300);

  exploreInput.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
  });

  exploreInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
      performSearch(serviceProvidersData, exploreInput.value, providersGrid, filterButtons, renderProviderCards);
    }
  });

  initializeOverlayVoiceSearch(exploreInput, (query) => {
    performSearch(serviceProvidersData, query, providersGrid, filterButtons, renderProviderCards);
  });

  setTimeout(() => overlay.classList.add('active'), 10);


  const style = document.createElement('style');
  style.textContent = `
  .providers-overlay {
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  bottom: 60px;
  background: #1a1a1f;
  z-index: 1000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  overflow-y: auto;
  width: 100%;
  display: flex;
  flex-direction: column;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.providers-overlay::-webkit-scrollbar {
  display: none;
}

.providers-overlay.active {
  opacity: 1;
  visibility: visible;
}

.providers-content {
  flex: 1;
  width: 100%;
  max-width: 100%;
  padding: 20px;
  margin: 0 auto;
  position: relative;
}

.providers-header {
  padding: 16px 24px;
  background: rgba(26, 26, 31, 0.95);
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 30;
  border-bottom: 1px solid rgba(255, 215, 0, 0.15);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  height: 72px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
}

.providers-header h2 {
  color: var(--primary-color);
  margin: 0;
  font-size: 1.75em;
  font-weight: 600;
  letter-spacing: -0.02em;
  display: flex;
  align-items: center;
  gap: 12px;
}

.providers-header h2::before {
  content: '';
  display: inline-block;
  width: 24px;
  height: 24px;
  background: var(--primary-color);
  mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpath d='M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z'%3E%3C/path%3E%3Ccircle cx='12' cy='10' r='3'%3E%3C/circle%3E%3C/svg%3E");
  mask-repeat: no-repeat;
  mask-position: center;
  mask-size: contain;
}


.close-providers {
  background: rgba(255, 215, 0, 0.1);
  border: 1px solid rgba(255, 215, 0, 0.2);
  color: var(--primary-color);
  font-size: 1.25em;
  cursor: pointer;
  padding: 8px 16px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.close-providers:hover {
  background: rgba(255, 215, 0, 0.15);
  transform: translateY(-1px);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.close-providers:active {
  transform: translateY(0);
}

.providers-filter-container {
  position: fixed;
  top: 70px;
  left: 0;
  width: 100vw;
  max-width: 100vw;
  z-index: 10;
  background: #1a1a1f;
  margin-left: 10px;
  padding: 10px 15px;
  box-sizing: border-box;
}

.providers-filter-buttons {
  display: flex;
  justify-content: flex-start;
  gap: 4px;
  padding-bottom: 10px;
  width: 100vw;
  margin-left: calc(-50vw + 50%);
  overflow-x: auto;
  scrollbar-width: none;
}

.providers-filter-buttons::-webkit-scrollbar {
  height: 0px;
}

.providers-filter-buttons::-webkit-scrollbar-thumb {
  background-color: var(--primary-color);
  border-radius: 3px;
}

.filter-button {
  margin-top: 2px;
  margin-left: 2px;
  margin-right: 5px;
  flex-shrink: 0;
  background: rgba(255, 215, 0, 0.1);
  color: var(--text-light);
  border: 1px solid rgba(255, 215, 0, 0.2);
  padding: 10px 15px;
  border-radius: 25px;
  transition: all 0.3s ease;
  cursor: pointer;
  white-space: nowrap;
  font-weight: 500;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.filter-button:hover {
  background: var(--primary-color);
  color: black;
  transform: scale(1.05);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.filter-button.active {
  background: var(--primary-color);
  color: black;
  font-weight: 700;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
}

.providers-grid-container {
  flex: 1;
  width: 100%;
  padding-bottom: 20px;
  position: relative;
}

.providers-grid {
  margin-top: 70px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
  gap: 20px;
  animation: fadeInUp 0.5s ease-out;
  padding-bottom: 140px;
  width: 100%;
}

.provider-card {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 16px;
  overflow: hidden;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  cursor: pointer;
}

.provider-card:hover {
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  border-color: rgba(255, 215, 0, 0.3);
}

.provider-card-header {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 20px;
  background: rgba(255, 255, 255, 0.03);
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
}

.brand-logo {
  width: 60px;
  height: 60px;
  border-radius: 12px;
  object-fit: cover;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.provider-card-title {
  flex: 1;
}

.provider-card-title h3 {
  margin: 0;
  font-size: 1.2em;
  color: #fff;
  font-weight: 600;
}

.category-tag {
  margin: 5px 0 0;
  font-size: 0.9em;
  color: rgba(255, 215, 0, 0.8);
  font-weight: 500;
}

.provider-card-body {
  padding: 20px;
}

.provider-description {
  margin: 0;
  font-size: 0.95em;
  color: rgba(255, 255, 255, 0.8);
  line-height: 1.5;
}

.provider-card-actions {
  display: flex;
  gap: 10px;
  padding: 15px 20px;
  background: rgba(255, 255, 255, 0.03);
  border-top: 1px solid rgba(255, 255, 255, 0.05);
}

.action-button {
  flex: 1;
  background: rgba(255, 215, 0, 0.1);
  border: none;
  border-radius: 8px;
  padding: 10px;
  color: #fff;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-size: 0.9em;
}

.action-button:hover {
  background: rgba(255, 215, 0, 0.2);
  transform: translateY(-2px);
}

.action-button i {
  font-size: 1.1em;
}

/* Responsive Design */
@media (max-width: 768px) {
  .provider-card-header {
    padding: 15px;
  }

  .brand-logo {
    width: 50px;
    height: 50px;
  }

  .provider-card-title h3 {
    font-size: 1.1em;
  }

  .provider-card-body {
    padding: 15px;
  }

  .provider-card-actions {
    padding: 10px 15px;
    flex-wrap: wrap;
  }

  .action-button {
    flex: 1 1 45%;
    padding: 8px;
    font-size: 0.85em;
  }
}

  .chat-container {
    position: fixed;
    bottom: 60px;
    left: 0;
    right: 0;
    width: 100%;
    max-width: 100%;
    z-index: 1000;
    padding: 20px;
    background: #1a1a1f;
    border-top: 1px solid rgba(255, 255, 255, 0.1);
    display: flex;
    gap: 12px;
    align-items: center;
    box-sizing: border-box;
  }

  .input-container {
    flex: 1;
    position: relative;
    display: flex;
    align-items: center;
  }

  #providersExploreInput {
    flex: 1;
    background: rgba(255, 255, 255, 0.05);
    border: 1px solid rgba(255, 215, 0, 0.2);
    border-radius: 12px;
    padding: 12px 50px 12px 20px;
    color: #fff;
    font-size: 0.95em;
    width: 100%;
    transition: border-color 0.3s ease;
  }

  #providersExploreInput:focus {
    border-color: rgba(255, 255, 255, 0.5);
  }

  .voice-search {
    position: absolute;
    right: 10px;
    background: none;
    border: none;
    color: var(--primary-color);
    cursor: pointer;
    padding: 10px;
    transition: all 0.3s ease;
}

  .voice-search:hover {
    opacity: 0.8;
  }

  .voice-search.active {
    color: #ffd700;
    animation: pulse 1.5s infinite;
  }

  .voice-search i { 
    font-size: 1.5em; 
}

  @keyframes pulse {
    0% { transform: scale(1); }
    50% { transform: scale(1.1); }
    100% { transform: scale(1); }
  }

/* Desktop Specific Styles */
@media (min-width: 769px) {
  .chat-container {
    padding: 24px;
    bottom: 60px;
  }

  .input-container {
    max-width: 800px;
    margin: 0 auto;
  }

  #providersExploreInput {
    height: 80px;
    font-size: 1.05rem;
    padding: 0 50px 0 24px;
    border-radius: 16px;
  }

  .voice-search {
    width: 50px;
    height: 50px;
    right: 8px;
  }

  .voice-search i {
    font-size: 2.1em;
  }
}

/* Tablet Specific Styles */
@media (min-width: 769px) and (max-width: 1024px) {
  .input-container {
    max-width: 700px;
  }
}

/* Desktop and laptop styles */
@media (min-width: 769px) {
  .chat-container {
    width: 100%;
    display: flex;
    justify-content: center;
  }
  
  #providersExploreInput {
    width: 60%;
    max-width: 1200px;
  }
  
  .providers-grid {
    padding-bottom: 180px;
  }
}

/* Mobile styles */
@media (max-width: 768px) {
  .chat-container {
    width: 100%;
    left: 0;
    transform: none;
    padding: 10px;
    gap: 8px;
  }

  .providers-grid {
    grid-template-columns: 1fr;
    padding: 0 10px;
    margin-top: 80px;
    gap: 15px;
  }

  .provider-card {
    width: 100%;
    margin: 0;
  }

  .providers-grid-container {
    width: 100%;
    max-width: 100%;
    padding: 0;
  }

  .providers-content {
    padding: 10px 0;
    padding-bottom: 200px;
  }

  #providersExploreInput {
    width: 100%;
    padding: 20px 20px;
  }
}
   `;
  document.head.appendChild(style);
}

function renderProviderCards(filteredData) {
  return filteredData.map(provider => `
    <div class="provider-card" onclick="openProviderPage('${provider.category}')">
      <div class="provider-card-header">
        <img src="${provider.details.image}" alt="${provider.details.name}" class="brand-logo">
        <div class="provider-card-title">
          <h3>${provider.details.name}</h3>
          <p class="category-tag">${provider.category}</p>
        </div>
      </div>
      <div class="provider-card-body">
        <p class="provider-description">${provider.details.description}</p>
      </div>
      <div class="provider-card-actions">
        ${provider.details.services ? `
          <a href="${provider.details.serviceLink}" target="_blank" class="action-button" onclick="event.stopPropagation()">
            <i class="fas fa-tools"></i> Services
          </a>
        ` : ''}
        <a href="${provider.details.mapLink}" target="_blank" class="action-button" onclick="event.stopPropagation()">
          <i class="fas fa-map-marker-alt"></i> Map
        </a>
        <button class="action-button" onclick="openProviderPage('${provider.category}'); event.stopPropagation()">
          <i class="fas fa-info-circle"></i> Info
        </button>
        <!-- Chat Button for Plumber -->
        <button class="action-button chat-button" onclick="showChatSupport('${provider.details.name}'); event.stopPropagation()">
          <i class="fas fa-comments"></i> Chat
        </button>
      </div>
    </div>
  `).join('');
}

function openProviderPage(category) {
  const provider = serviceProvidersData.find(prov => prov.category === category);
  if (!provider) return;

  const providerPage = document.createElement('div');
  providerPage.className = 'provider-page';
  providerPage.innerHTML = `
    <div class="provider-header">
      <button class="back-button" onclick="closeProviderPage()">
        <i class="fas fa-arrow-left"></i> Back
      </button>
      <h2>${provider.details.name}</h2>
    </div>
    <div class="provider-content">
      <!-- Provider Info -->
      <div class="provider-info">
        <img src="${provider.details.image}" alt="${provider.details.name}" class="provider-logo">
        <h3>${provider.details.name}</h3>
        <p class="provider-description">${provider.details.description}</p>
        <div class="social-media">
          <a href="${provider.details.social?.facebook}" target="_blank"><i class="fab fa-facebook"></i></a>
          <a href="${provider.details.social?.twitter}" target="_blank"><i class="fab fa-twitter"></i></a>
          <a href="${provider.details.social?.instagram}" target="_blank"><i class="fab fa-instagram"></i></a>
        </div>
      </div>
      
      <!-- Tabs -->
      <div class="provider-tabs">
        <button class="tab-button active" data-tab="locator">Service Locator</button>
        <button class="tab-button" data-tab="services">Services Offered</button>
        <button class="tab-button" data-tab="promos">Promos & Deals</button>
        <button class="tab-button" data-tab="support">Customer Support</button>
      </div>
      
      <!-- Tab Content -->
      <div class="tab-content">
        <div id="locator" class="tab-pane active">
          <!-- Service Locator Content -->
          <div class="service-locator">
            <h4>Find Services</h4>
            <div class="locator-grid" id="locatorGrid">
              ${renderOutlets(provider.details.outlets)}
            </div>
          </div>
        </div>
        <div id="services" class="tab-pane">
          <!-- Services Offered Content -->
          <div class="services-offered">
            <h4>Available Services</h4>
            <div class="services-grid" id="servicesGrid">
              ${provider.details.services?.map(service => `
                <div class="service-item">
                  <img src="${service.image}" alt="${service.name}">
                  <h5>${service.name}</h5>
                  <p>${service.price}</p>
                  <button class="book-service" onclick="bookService('${service.name}', '${service.price}')">
                    <i class="fas fa-calendar-check"></i> Book Service
                  </button>
                </div>
              `).join('')}
            </div>
          </div>
        </div>
        <div id="promos" class="tab-pane">
          <!-- Promos & Deals Content -->
          <div class="promos-deals" id="promosGrid">
            <h4>Available Promos & Deals</h4>
            ${provider.details.promos?.map(promo => `
              <div class="promo-item">
                <p>${promo.code} - ${promo.description}</p>
                <button onclick="copyPromo('${promo.code}')">Copy Code</button>
              </div>
            `).join('')}
          </div>
        </div>
        <div id="support" class="tab-pane">
          <!-- Customer Support Content -->
          <div class="customer-support" id="supportContent">
            <h4>Contact Support</h4>
            <p>Email: ${provider.details.support?.email}</p>
            <p>Phone: ${provider.details.support?.phone}</p>
            <button onclick="showChatSupport('${provider.category}')">
              <i class="fas fa-comments"></i> Chat with Us
            </button>
          </div>
        </div>
      </div>
      
      <!-- Search Container -->
      <div class="chat-container">
        <input type="text" id="providerInput" placeholder="Search for services, products, or locations..." />
        <div class="input-actions">
          <button class="send-message" id="providerButton">
            <i class="fas fa-paper-plane"></i>
          </button>
        </div>
      </div>
    </div>
  `;

  document.body.appendChild(providerPage);
  addProviderPageStyles();
  initializeTabs();
  initializeSearch();
  scrollToTop();
}

function renderOutlets(outlets) {
  return outlets.map(outlet => `
    <div class="outlet-card">
      <h5>${outlet.name}</h5>
      <p>${outlet.address}</p>
      <p>${outlet.timings}</p>
      <a href="${outlet.mapLink}" target="_blank" class="action-button">
        <i class="fas fa-map-marker-alt"></i> View on Map
      </a>
    </div>
  `).join('');
}

// Function to scroll to the top of the provider page
function scrollToTop() {
  const providerContent = document.querySelector('.provider-content');
  if (providerContent) {
    providerContent.scrollTo({ top: 0, behavior: 'smooth' });
  }
}

// Function to initialize search functionality with debounce
function initializeSearch() {
  const providerInput = document.getElementById('providerInput');
  if (providerInput) {
    providerInput.addEventListener('input', debounce((e) => {
      const searchTerm = e.target.value.toLowerCase().trim();
      const intent = understandUserIntent(searchTerm);
      filterProviderContent(searchTerm, intent);
    }, 300));
  }
}

// Function to understand user intent using simple NLP
function understandUserIntent(searchTerm) {
  if (searchTerm.includes('offer') || searchTerm.includes('deal')) {
    return 'promos';
  } else if (searchTerm.includes('service') || searchTerm.includes('item')) {
    return 'services';
  } else if (searchTerm.includes('support') || searchTerm.includes('contact')) {
    return 'support';
  } else if (searchTerm.includes('store') || searchTerm.includes('location')) {
    return 'locator';
  }
  return null;
}

// Function to filter provider content based on search term and intent
function filterProviderContent(searchTerm, intent) {
  const providerContent = document.querySelector('.provider-content');
  const tabPanes = document.querySelectorAll('.tab-pane');
  const noResultsMessage = document.getElementById('noResultsMessage');

  if (!searchTerm) {
    // If search term is empty, show all content
    tabPanes.forEach(pane => pane.style.display = 'block');
    if (noResultsMessage) noResultsMessage.remove();
    return;
  }

  // Hide all tab panes initially
  tabPanes.forEach(pane => pane.style.display = 'none');

  // Switch to the relevant tab based on intent
  if (intent) {
    document.querySelector(`[data-tab="${intent}"]`).click();
  }

  // Filter Services Offered
  const servicesGrid = document.getElementById('servicesGrid');
  if (servicesGrid) {
    const services = servicesGrid.querySelectorAll('.service-item');
    let serviceMatch = false;
    services.forEach(service => {
      const serviceName = service.querySelector('h5').textContent.toLowerCase();
      const serviceDescription = service.querySelector('p').textContent.toLowerCase();
      if (serviceName.includes(searchTerm) || serviceDescription.includes(searchTerm)) {
        service.style.display = 'block';
        serviceMatch = true;
      } else {
        service.style.display = 'none';
      }
    });
    if (serviceMatch) {
      document.getElementById('services').style.display = 'block';
    }
  }

  // Filter Promos & Deals
  const promosGrid = document.getElementById('promosGrid');
  if (promosGrid) {
    const promos = promosGrid.querySelectorAll('.promo-item');
    let promoMatch = false;
    promos.forEach(promo => {
      const promoText = promo.textContent.toLowerCase();
      if (promoText.includes(searchTerm)) {
        promo.style.display = 'flex';
        promoMatch = true;
      } else {
        promo.style.display = 'none';
      }
    });
    if (promoMatch) {
      document.getElementById('promos').style.display = 'block';
    }
  }

  // Filter Customer Support
  const supportContent = document.getElementById('supportContent');
  if (supportContent) {
    const supportText = supportContent.textContent.toLowerCase();
    if (supportText.includes(searchTerm)) {
      document.getElementById('support').style.display = 'block';
    }
  }

  // Filter Service Locator
  const locatorGrid = document.getElementById('locatorGrid');
  if (locatorGrid) {
    const locators = locatorGrid.querySelectorAll('.locator-card');
    let locatorMatch = false;
    locators.forEach(locator => {
      const locatorText = locator.textContent.toLowerCase();
      if (locatorText.includes(searchTerm)) {
        locator.style.display = 'block';
        locatorMatch = true;
      } else {
        locator.style.display = 'none';
      }
    });
    if (locatorMatch) {
      document.getElementById('locator').style.display = 'block';
    }
  }

  // Show no results message if no matches found
  if (!serviceMatch && !promoMatch && !supportText?.includes(searchTerm) && !locatorMatch) {
    if (!noResultsMessage) {
      const noResultsDiv = document.createElement('div');
      noResultsDiv.id = 'noResultsMessage';
      noResultsDiv.textContent = 'No results found.';
      noResultsDiv.style.textAlign = 'center';
      noResultsDiv.style.color = 'rgba(255, 255, 255, 0.8)';
      noResultsDiv.style.marginTop = '20px';
      providerContent.appendChild(noResultsDiv);
    }
  } else {
    if (noResultsMessage) noResultsMessage.remove();
  }

  // Scroll to the top of the provider page
  scrollToTop();
}

// Function to close the provider page
function closeProviderPage() {
  const providerPage = document.querySelector('.provider-page');
  if (providerPage) {
    providerPage.remove();
  }
}

// Function to initialize tabs on the provider page
function initializeTabs() {
  const tabButtons = document.querySelectorAll('.tab-button');
  const tabPanes = document.querySelectorAll('.tab-pane');

  tabButtons.forEach(button => {
    button.addEventListener('click', () => {
      const tabName = button.getAttribute('data-tab');
      tabButtons.forEach(btn => btn.classList.remove('active'));
      tabPanes.forEach(pane => pane.classList.remove('active'));
      button.classList.add('active');
      document.getElementById(tabName).classList.add('active');
    });
  });
}

function addProviderPageStyles() {
  const style = document.createElement('style');
  style.textContent = `
    /* Provider Page Container */
    .provider-page {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: #1a1a1f;
      z-index: 2000;
      overflow-y: auto;
      padding: 20px;
      box-sizing: border-box;
      font-family: 'Inter', sans-serif;
    }

    /* Provider Header */
    .provider-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 20px;
      padding: 15px;
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      backdrop-filter: blur(10px);
    }

    .back-button {
      background: none;
      border: none;
      color: #fff;
      font-size: 1.5em;
      cursor: pointer;
      transition: transform 0.2s ease, opacity 0.2s ease;
    }

    .back-button:hover {
      transform: scale(1.1);
      opacity: 0.8;
    }

    .provider-header h2 {
      margin: 0;
      font-size: 1.5em;
      color: #fff;
      font-weight: 600;
      letter-spacing: -0.5px;
    }

    /* Provider Info */
    .provider-info {
      text-align: center;
      margin-bottom: 20px;
    }

    .provider-logo {
      width: 100px;
      height: 100px;
      border-radius: 50%;
      object-fit: cover;
      border: 3px solid rgba(255, 215, 0, 0.5);
      margin-bottom: 15px;
      transition: transform 0.3s ease;
    }

    .provider-logo:hover {
      transform: scale(1.05);
    }

    .provider-info h3 {
      margin: 0;
      font-size: 1.75em;
      color: #fff;
      font-weight: 600;
      letter-spacing: -0.5px;
    }

    .provider-description {
      color: rgba(255, 255, 255, 0.8);
      font-size: 1em;
      margin-top: 10px;
      line-height: 1.5;
    }

    /* Social Media Icons */
    .social-media {
      display: flex;
      justify-content: center;
      gap: 20px;
      margin-top: 20px;
    }

    .social-media a {
      color: #fff;
      font-size: 1.75em;
      transition: transform 0.3s ease, color 0.3s ease;
    }

    .social-media a:hover {
      color: #ffd700;
      transform: scale(1.2);
    }

    /* Tabs */
    .provider-tabs {
      display: flex;
      justify-content: space-around;
      margin-bottom: 20px;
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      padding: 10px;
      backdrop-filter: blur(10px);
    }

    .tab-button {
      background: none;
      border: none;
      color: rgba(255, 255, 255, 0.7);
      font-size: 1em;
      cursor: pointer;
      padding: 12px 24px;
      border-radius: 12px;
      transition: all 0.3s ease;
    }

    .tab-button.active {
      background: rgba(255, 215, 0, 0.1);
      color: #ffd700;
      font-weight: 600;
    }

    .tab-button:hover {
      background: rgba(255, 215, 0, 0.1);
    }

    /* Tab Content */
    .tab-content {
      padding-bottom: 200px;
    }

    .tab-pane {
      display: none;
      animation: fadeIn 0.5s ease;
    }

    .tab-pane.active {
      display: block;
    }

    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }

    /* Services Offered */
    .services-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
      gap: 20px;
    }

    .service-item {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      padding: 20px;
      text-align: center;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      backdrop-filter: blur(10px);
    }

    .service-item:hover {
      transform: translateY(-5px);
      box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
    }

    .service-item img {
      width: 100%;
      height: auto;
      border-radius: 12px;
      margin-bottom: 15px;
      transition: transform 0.3s ease;
    }

    .service-item:hover img {
      transform: scale(1.05);
    }

    .service-item h5 {
      margin: 0;
      font-size: 1.1em;
      color: #fff;
      font-weight: 600;
    }

    .service-item p {
      margin: 10px 0;
      color: rgba(255, 255, 255, 0.8);
      font-size: 0.95em;
    }

    .book-service {
      background: rgba(255, 215, 0, 0.1);
      border: none;
      color: #fff;
      padding: 10px 20px;
      border-radius: 12px;
      cursor: pointer;
      transition: background 0.3s ease, transform 0.3s ease;
    }

    .book-service:hover {
      background: rgba(255, 215, 0, 0.2);
      transform: scale(1.05);
    }

    /* Promos & Deals */
    .promos-deals {
      display: grid;
      gap: 20px;
    }

    .promo-item {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      padding: 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      backdrop-filter: blur(10px);
    }

    .promo-item p {
      margin: 0;
      color: rgba(255, 255, 255, 0.8);
      font-size: 1em;
    }

    .promo-item button {
      background: rgba(255, 215, 0, 0.1);
      border: none;
      color: #fff;
      padding: 10px 20px;
      border-radius: 12px;
      cursor: pointer;
      transition: background 0.3s ease, transform 0.3s ease;
    }

    .promo-item button:hover {
      background: rgba(255, 215, 0, 0.2);
      transform: scale(1.05);
    }

    /* Customer Support */
    .customer-support {
      text-align: center;
      padding: 20px;
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      backdrop-filter: blur(10px);
    }

    .customer-support p {
      margin: 10px 0;
      color: rgba(255, 255, 255, 0.8);
      font-size: 1em;
    }

    .customer-support button {
      background: rgba(255, 215, 0, 0.1);
      border: none;
      color: #fff;
      padding: 12px 24px;
      border-radius: 12px;
      cursor: pointer;
      transition: background 0.3s ease, transform 0.3s ease;
    }

    .customer-support button:hover {
      background: rgba(255, 215, 0, 0.2);
      transform: scale(1.05);
    }

    /* Chat Container */
    .chat-container {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      width: 100%;
      max-width: 100%;
      z-index: 1000;
      padding: 20px;
      background: #1a1a1f;
      border-top: 1px solid rgba(255, 215, 0, 0.1);
      display: flex;
      gap: 12px;
      align-items: center;
      box-sizing: border-box;
      backdrop-filter: blur(10px);
    }

    #providerInput {
      flex: 1;
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid rgba(255, 215, 0, 0.2);
      border-radius: 12px;
      padding: 20px;
      color: #fff;
      font-size: 1em;
      width: 100%;
      transition: border-color 0.3s ease;
    }

    #providerInput:focus {
      border-color: rgba(255, 215, 0, 0.5);
    }

        /* Outlet Card Styles */
    .outlet-card {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      padding: 20px;
      margin-bottom: 20px;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      backdrop-filter: blur(10px);
    }

    .outlet-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
    }

    .outlet-card h5 {
      margin: 0;
      font-size: 1.1em;
      color: #fff;
      font-weight: 600;
    }

    .outlet-card p {
      margin: 10px 0;
      color: rgba(255, 255, 255, 0.8);
      font-size: 0.95em;
    }

    .outlet-card .action-button {
      background: rgba(255, 215, 0, 0.1);
      border: none;
      color: #fff;
      padding: 10px 20px;
      border-radius: 12px;
      cursor: pointer;
      transition: background 0.3s ease, transform 0.3s ease;
      display: inline-block;
      margin-top: 10px;
    }

    .outlet-card .action-button:hover {
      background: rgba(255, 215, 0, 0.2);
      transform: scale(1.05);
    }

    /* Responsive Design */
    @media (max-width: 768px) {
      .provider-header h2 {
        font-size: 1.2em;
      }

      .provider-logo {
        width: 80px;
        height: 80px;
      }

      .provider-description {
        font-size: 0.9em;
      }

      .tab-button {
        font-size: 0.9em;
        padding: 10px 20px;
      }

      .services-grid {
        grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
      }

      .service-item h5 {
        font-size: 1em;
      }

      .service-item p {
        font-size: 0.85em;
      }

      .book-service {
        font-size: 0.85em;
      }

      .promo-item p {
        font-size: 0.9em;
      }

      .promo-item button {
        font-size: 0.85em;
      }

      .customer-support button {
        font-size: 0.9em;
      }
    }

    /* Small Mobile Optimizations */
    @media (max-width: 480px) {
      .provider-page {
        padding: 10px;
      }

      .services-grid {
        grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
        gap: 10px;
      }

      .tab-button {
        padding: 8px 16px;
        font-size: 0.85em;
      }

      .chat-container {
        padding: 20px;
      }

      #providerInput {
        padding: 20px 20px;
      }
    }
  `;
  document.head.appendChild(style);
}

// Function to filter providers based on category
function filterProviders(category) {
  const providersGrid = document.getElementById('providersGrid');
  let filteredData = serviceProvidersData;

  if (category !== 'All') {
    filteredData = filteredData.filter(provider => provider.category === category);
  }

  providersGrid.innerHTML = renderProviderCards(filteredData);

  const filterButtons = document.querySelectorAll('.filter-button');
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === category) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

// Additional placeholder functions (you might want to implement these)
function selectProvider(category) {
  console.log(`Selected provider in category: ${category}`);
}

function selectProvider(category) {
  // Optional: Add any specific action when a provider card is clicked
  console.log(`Selected provider in category: ${category}`);
}

const publicPlacesData = [
  // Delhi
  {
    category: "Park",
    metropolis: "Delhi",
    details: {
      name: "Lodhi Garden",
      image: "https://example.com/lodhi-garden.jpg",
      description: "A serene park with historical monuments and lush greenery.",
      address: "Lodhi Road, Delhi",
      timings: "6 AM - 8 PM",
      entryFee: "Free",
      mapLink: "https://maps.example.com/lodhi-garden",
      website: "https://www.delhitourism.gov.in/lodhi-garden",
      images: [
        "https://example.com/lodhi-garden1.jpg",
        "https://example.com/lodhi-garden2.jpg"
      ],
      social: {
        facebook: "https://facebook.com/lodhigarden",
        twitter: "https://twitter.com/lodhigarden",
        instagram: "https://instagram.com/lodhigarden"
      },
      facilities: [
        { name: "Walking Trails", description: "Well-maintained walking paths" },
        { name: "Picnic Spots", description: "Designated areas for picnics" }
      ],
      events: [
        { name: "Yoga Sessions", date: "Every Sunday, 7 AM" }
      ],
      support: {
        email: "info@lodhigarden.com",
        phone: "+91-1234567890"
      }
    }
  },
  {
    category: "Museum",
    metropolis: "Delhi",
    details: {
      name: "National Museum",
      image: "https://example.com/national-museum.jpg",
      description: "Explore India's rich history and cultural heritage.",
      address: "Janpath, Delhi",
      timings: "10 AM - 6 PM",
      entryFee: "$5",
      mapLink: "https://maps.example.com/national-museum",
      website: "https://www.nationalmuseumindia.gov.in",
      images: [
        "https://example.com/national-museum1.jpg",
        "https://example.com/national-museum2.jpg"
      ],
      social: {
        facebook: "https://facebook.com/nationalmuseum",
        twitter: "https://twitter.com/nationalmuseum",
        instagram: "https://instagram.com/nationalmuseum"
      },
      facilities: [
        { name: "Guided Tours", description: "Available in multiple languages" },
        { name: "Cafeteria", description: "Serves snacks and beverages" }
      ],
      events: [
        { name: "Art Exhibition", date: "15th October, 2023" }
      ],
      support: {
        email: "info@nationalmuseum.com",
        phone: "+91-9876543210"
      }
    }
  },
  // Mumbai
  {
    category: "Beach",
    metropolis: "Mumbai",
    details: {
      name: "Juhu Beach",
      image: "https://example.com/juhu-beach.jpg",
      description: "A popular beach with scenic views and street food.",
      address: "Juhu, Mumbai",
      timings: "Open 24 hours",
      entryFee: "Free",
      mapLink: "https://maps.example.com/juhu-beach",
      website: "https://www.mumbaitourism.gov.in/juhu-beach",
      images: [
        "https://example.com/juhu-beach1.jpg",
        "https://example.com/juhu-beach2.jpg"
      ],
      social: {
        facebook: "https://facebook.com/juhubeach",
        twitter: "https://twitter.com/juhubeach",
        instagram: "https://instagram.com/juhubeach"
      },
      facilities: [
        { name: "Food Stalls", description: "Local street food vendors" },
        { name: "Parking", description: "Ample parking space available" }
      ],
      events: [
        { name: "Beach Festival", date: "20th November, 2023" }
      ],
      support: {
        email: "info@juhubeach.com",
        phone: "+91-9876543210"
      }
    }
  },
  // Bangalore
  {
    category: "Library",
    metropolis: "Bangalore",
    details: {
      name: "State Central Library",
      image: "https://example.com/state-library.jpg",
      description: "A historic library with a vast collection of books.",
      address: "Cubbon Park, Bangalore",
      timings: "9 AM - 7 PM",
      entryFee: "Free",
      mapLink: "https://maps.example.com/state-library",
      website: "https://www.bangalorelibrary.gov.in",
      images: [
        "https://example.com/state-library1.jpg",
        "https://example.com/state-library2.jpg"
      ],
      social: {
        facebook: "https://facebook.com/statelibrary",
        twitter: "https://twitter.com/statelibrary",
        instagram: "https://instagram.com/statelibrary"
      },
      facilities: [
        { name: "Reading Rooms", description: "Quiet spaces for reading" },
        { name: "Wi-Fi", description: "Free Wi-Fi for visitors" }
      ],
      events: [
        { name: "Book Fair", date: "5th December, 2023" }
      ],
      support: {
        email: "info@statelibrary.com",
        phone: "+91-9876543210"
      }
    }
  }
];

// Function to show the places overlay
function showPlacesOverlay() {
  let overlay = document.getElementById('placesOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'placesOverlay';
    overlay.className = 'places-overlay';
    document.body.appendChild(overlay);
  }

  const categories = ['All', ...new Set(publicPlacesData.map(place => place.category))];

  overlay.innerHTML = `
    <div class="places-content">
      <div class="places-header">
        <h2>Public Places</h2>
        <button class="close-places" onclick="hidePlacesOverlay()">
          <i class="fas fa-long-arrow-alt-left"></i>
        </button>
      </div>
      
      <div class="places-filter-container">
        <div class="places-filter-buttons">
          ${categories.map(category => `
            <button 
              class="filter-button ${category === 'All' ? 'active' : ''}" 
              data-category="${category}" 
              onclick="filterPlaces('${category}')"
            >
              ${category}
            </button>
          `).join('')}
        </div>
      </div>

      <div class="places-grid" id="placesGrid">
        ${renderPlaceCards(publicPlacesData)}
      </div>
    </div>

    <div class="chat-container">
      <div class="input-container">
        <input type="text" id="placesExploreInput" placeholder="Search for services or locations..." />
        <button class="voice-search" id="voiceSearchButton">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  const exploreInput = document.getElementById('placesExploreInput');
  const placesGrid = document.getElementById('placesGrid');
  const filterButtons = document.querySelectorAll('.filter-button');

  const debouncedSearch = debounce((query) => {
    performSearch(publicPlacesData, query, placesGrid, filterButtons, renderPlaceCards);
  }, 300);

  exploreInput.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
  });

  exploreInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
      performSearch(publicPlacesData, exploreInput.value, placesGrid, filterButtons, renderPlaceCards);
    }
  });

  initializeOverlayVoiceSearch(exploreInput, (query) => {
    performSearch(publicPlacesData, query, placesGrid, filterButtons, renderPlaceCards);
  });

  setTimeout(() => overlay.classList.add('active'), 10);


  const style = document.createElement('style');
  style.textContent = `
  .places-overlay {
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  bottom: 60px;
  background: #1a1a1f;
  z-index: 1000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  overflow-y: auto;
  width: 100%;
  display: flex;
  flex-direction: column;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.places-overlay::-webkit-scrollbar {
  display: none;
}

.places-overlay.active {
  opacity: 1;
  visibility: visible;
}

.places-content {
  flex: 1;
  width: 100%;
  max-width: 100%;
  padding: 20px;
  margin: 0 auto;
  position: relative;
}

.places-header {
  padding: 16px 24px;
  background: rgba(26, 26, 31, 0.95);
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 30;
  border-bottom: 1px solid rgba(255, 215, 0, 0.15);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  height: 72px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
}

.places-header h2 {
  color: var(--primary-color);
  margin: 0;
  font-size: 1.75em;
  font-weight: 600;
  letter-spacing: -0.02em;
  display: flex;
  align-items: center;
  gap: 12px;
}

.places-header h2::before {
  content: '';
  display: inline-block;
  width: 24px;
  height: 24px;
  background: var(--primary-color);
  mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpath d='M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z'%3E%3C/path%3E%3Ccircle cx='12' cy='10' r='3'%3E%3C/circle%3E%3C/svg%3E");
  mask-repeat: no-repeat;
  mask-position: center;
  mask-size: contain;
}

.header-buttons-container {
  display: flex;
  align-items: center;
  gap: 16px;
}

.close-places {
  background: rgba(255, 215, 0, 0.1);
  border: 1px solid rgba(255, 215, 0, 0.2);
  color: var(--primary-color);
  font-size: 1.25em;
  cursor: pointer;
  padding: 8px 16px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.close-places:hover {
  background: rgba(255, 215, 0, 0.15);
  transform: translateY(-1px);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.close-places:active {
  transform: translateY(0);
}

.places-filter-container {
  position: fixed;
  top: 70px;
  left: 0;
  width: 100vw;
  max-width: 100vw;
  z-index: 10;
  background: #1a1a1f;
  margin-left: 10px;
  padding: 10px 15px;
  box-sizing: border-box;
}

.places-filter-buttons {
  display: flex;
  justify-content: flex-start;
  gap: 4px;
  padding-bottom: 10px;
  width: 100vw;
  margin-left: calc(-50vw + 50%);
  overflow-x: auto;
  scrollbar-width: none;
}

.places-filter-buttons::-webkit-scrollbar {
  height: 0px;
}

.places-filter-buttons::-webkit-scrollbar-thumb {
  background-color: var(--primary-color);
  border-radius: 3px;
}

.filter-button {
  margin-top: 2px;
  margin-left: 2px;
  margin-right: 5px;
  flex-shrink: 0;
  background: rgba(255, 215, 0, 0.1);
  color: var(--text-light);
  border: 1px solid rgba(255, 215, 0, 0.2);
  padding: 10px 15px;
  border-radius: 25px;
  transition: all 0.3s ease;
  cursor: pointer;
  white-space: nowrap;
  font-weight: 500;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.filter-button:hover {
  background: var(--primary-color);
  color: black;
  transform: scale(1.05);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.filter-button.active {
  background: var(--primary-color);
  color: black;
  font-weight: 700;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
}

.places-grid-container {
  flex: 1;
  width: 100%;
  padding-bottom: 20px;
  position: relative;
}

.places-grid {
  margin-top: 70px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
  gap: 20px;
  animation: fadeInUp 0.5s ease-out;
  padding-bottom: 140px;
  width: 100%;
}

.place-card {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 16px;
  overflow: hidden;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  cursor: pointer;
}

.place-card:hover {
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  border-color: rgba(255, 215, 0, 0.3);
}

.place-card-header {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 20px;
  background: rgba(255, 255, 255, 0.03);
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
}

.brand-logo {
  width: 60px;
  height: 60px;
  border-radius: 12px;
  object-fit: cover;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.place-card-title {
  flex: 1;
}

.place-card-title h3 {
  margin: 0;
  font-size: 1.2em;
  color: #fff;
  font-weight: 600;
}

.category-tag {
  margin: 5px 0 0;
  font-size: 0.9em;
  color: rgba(255, 215, 0, 0.8);
  font-weight: 500;
}

.place-card-body {
  padding: 20px;
}

.place-description {
  margin: 0;
  font-size: 0.95em;
  color: rgba(255, 255, 255, 0.8);
  line-height: 1.5;
}

.place-card-actions {
  display: flex;
  gap: 10px;
  padding: 15px 20px;
  background: rgba(255, 255, 255, 0.03);
  border-top: 1px solid rgba(255, 255, 255, 0.05);
}

.action-button {
  flex: 1;
  background: rgba(255, 215, 0, 0.1);
  border: none;
  border-radius: 8px;
  padding: 10px;
  color: #fff;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-size: 0.9em;
}

.action-button:hover {
  background: rgba(255, 215, 0, 0.2);
  transform: translateY(-2px);
}

.action-button i {
  font-size: 1.1em;
}

/* Responsive Design */
@media (max-width: 768px) {
  .place-card-header {
    padding: 15px;
  }

  .brand-logo {
    width: 50px;
    height: 50px;
  }

  .place-card-title h3 {
    font-size: 1.1em;
  }

  .place-card-body {
    padding: 15px;
  }

  .place-card-actions {
    padding: 10px 15px;
    flex-wrap: wrap;
  }

  .action-button {
    flex: 1 1 45%;
    padding: 8px;
    font-size: 0.85em;
  }
}

  .chat-container {
    position: fixed;
    bottom: 60px;
    left: 0;
    right: 0;
    width: 100%;
    max-width: 100%;
    z-index: 1000;
    padding: 20px;
    background: #1a1a1f;
    border-top: 1px solid rgba(255, 255, 255, 0.1);
    display: flex;
    gap: 12px;
    align-items: center;
    box-sizing: border-box;
  }

  .input-container {
    flex: 1;
    position: relative;
    display: flex;
    align-items: center;
  }

  #placesExploreInput {
    flex: 1;
    background: rgba(255, 255, 255, 0.05);
    border: 1px solid rgba(255, 215, 0, 0.2);
    border-radius: 12px;
    padding: 12px 50px 12px 20px;
    color: #fff;
    font-size: 0.95em;
    width: 100%;
    transition: border-color 0.3s ease;
  }

  #placesExploreInput:focus {
    border-color: rgba(255, 255, 255, 0.5);
  }

  .voice-search {
    position: absolute;
    right: 10px;
    background: none;
    border: none;
    color: var(--primary-color);
    cursor: pointer;
    padding: 10px;
    transition: all 0.3s ease;
}

  .voice-search:hover {
    opacity: 0.8;
  }

  .voice-search.active {
    color: #ffd700;
    animation: pulse 1.5s infinite;
  }

  .voice-search i { 
    font-size: 1.5em; 
}

  @keyframes pulse {
    0% { transform: scale(1); }
    50% { transform: scale(1.1); }
    100% { transform: scale(1); }
  }

/* Desktop Specific Styles */
@media (min-width: 769px) {
  .chat-container {
    padding: 24px;
    bottom: 60px;
  }

  .input-container {
    max-width: 800px;
    margin: 0 auto;
  }

  #placesExploreInput {
    height: 80px;
    font-size: 1.05rem;
    padding: 0 50px 0 24px;
    border-radius: 16px;
  }

  .voice-search {
    width: 50px;
    height: 50px;
    right: 8px;
  }

  .voice-search i {
    font-size: 2.1em;
  }
}

/* Tablet Specific Styles */
@media (min-width: 769px) and (max-width: 1024px) {
  .input-container {
    max-width: 700px;
  }
}

/* Desktop and laptop styles */
@media (min-width: 769px) {
  .chat-container {
    width: 100%;
    display: flex;
    justify-content: center;
  }
  
  #placesExploreInput {
    width: 60%;
    max-width: 1200px;
  }
  
  .places-grid {
    padding-bottom: 180px;
  }
}

/* Mobile styles */
@media (max-width: 768px) {
  .chat-container {
    width: 100%;
    left: 0;
    transform: none;
    padding: 10px;
    gap: 8px;
  }

  .places-grid {
    grid-template-columns: 1fr;
    padding: 0 10px;
    margin-top: 80px;
    gap: 15px;
  }

  .place-card {
    width: 100%;
    margin: 0;
  }

  .places-grid-container {
    width: 100%;
    max-width: 100%;
    padding: 0;
  }

  .places-content {
    padding: 10px 0;
    padding-bottom: 200px;
  }

  #placesExploreInput {
    width: 100%;
    padding: 20px 20px;
  }
}
   `;
  document.head.appendChild(style);
}

function renderPlaceCards(filteredData) {
  return filteredData.map(place => `
    <div class="place-card" onclick="openPlacePage('${place.category}')">
      <div class="place-card-header">
        <img src="${place.details.image}" alt="${place.details.name}" class="brand-logo">
        <div class="place-card-title">
          <h3>${place.details.name}</h3>
          <p class="category-tag">${place.category}</p>
        </div>
      </div>
      <div class="place-card-body">
        <p class="place-description">${place.details.description}</p>
      </div>
      <div class="place-card-actions">
        <a href="${place.details.mapLink}" target="_blank" class="action-button" onclick="event.stopPropagation()">
          <i class="fas fa-map-marker-alt"></i> Map
        </a>
        <button class="action-button" onclick="openPlacePage('${place.category}'); event.stopPropagation()">
          <i class="fas fa-info-circle"></i> Info
        </button>
        <!-- Chat Button for Lodhi Garden -->
        <button class="action-button chat-button" onclick="showChatSupport('${place.details.name}'); event.stopPropagation()">
          <i class="fas fa-comments"></i> Chat
        </button>
      </div>
    </div>
  `).join('');
}

function openPlacePage(category) {
  const selectedMetropolis = localStorage.getItem('selectedMetropolis') || 'Select City';
  const place = publicPlacesData.find(pl => pl.category === category);
  if (!place) return;

  const placePage = document.createElement('div');
  placePage.className = 'place-page';
  placePage.innerHTML = `
    <div class="place-header">
      <button class="back-button" onclick="closePlacePage()">
        <i class="fas fa-arrow-left"></i> Back
      </button>
      <h2>${place.details.name}</h2>
    </div>
    <div class="place-content">
      <!-- Place Info -->
      <div class="place-info">
        <img src="${place.details.image}" alt="${place.details.name}" class="place-logo">
        <h3>${place.details.name}</h3>
        <p class="place-description">${place.details.description}</p>
        <div class="social-media">
          <a href="${place.details.social?.facebook}" target="_blank"><i class="fab fa-facebook"></i></a>
          <a href="${place.details.social?.twitter}" target="_blank"><i class="fab fa-twitter"></i></a>
          <a href="${place.details.social?.instagram}" target="_blank"><i class="fab fa-instagram"></i></a>
        </div>
      </div>
      
      <!-- Tabs -->
      <div class="place-tabs">
        <button class="tab-button active" data-tab="info">Info</button>
        <button class="tab-button" data-tab="facilities">Facilities</button>
        <button class="tab-button" data-tab="events">Events</button>
        <button class="tab-button" data-tab="support">Support</button>
      </div>
      
      <!-- Tab Content -->
      <div class="tab-content">
        <div id="info" class="tab-pane active">
          <!-- Basic Info -->
          <div class="basic-info">
            <h4>Address</h4>
            <p>${place.details.address}</p>
            <h4>Timings</h4>
            <p>${place.details.timings}</p>
            <h4>Entry Fee</h4>
            <p>${place.details.entryFee}</p>
          </div>
        </div>
        <div id="facilities" class="tab-pane">
          <!-- Facilities Content -->
          <div class="facilities-list">
            <h4>Facilities</h4>
            ${place.details.facilities?.map(facility => `
              <div class="facility-item">
                <h5>${facility.name}</h5>
                <p>${facility.description}</p>
              </div>
            `).join('')}
          </div>
        </div>
        <div id="events" class="tab-pane">
          <!-- Events Content -->
          <div class="events-list">
            <h4>Upcoming Events</h4>
            ${place.details.events?.map(event => `
              <div class="event-item">
                <h5>${event.name}</h5>
                <p>Date: ${event.date}</p>
              </div>
            `).join('')}
          </div>
        </div>
        <div id="support" class="tab-pane">
          <!-- Customer Support Content -->
          <div class="customer-support" id="supportContent">
            <h4>Contact Support</h4>
            <p>Email: ${place.details.support?.email}</p>
            <p>Phone: ${place.details.support?.phone}</p>
            <button onclick="showChatSupport('${place.category}')">
              <i class="fas fa-comments"></i> Chat with Us
            </button>
          </div>
        </div>
      </div>
      
      <!-- Search Container -->
      <div class="chat-container">
        <input type="text" id="placeInput" placeholder="Search for services, products, or locations..." />
        <div class="input-actions">
          <button class="send-message" id="placeButton">
            <i class="fas fa-paper-plane"></i>
          </button>
        </div>
      </div>
    </div>
  `;

  document.body.appendChild(placePage);
  addPlacePageStyles();
  initializeTabs();
  initializeSearch();
  scrollToTop();
}

// Function to close the place page
function closePlacePage() {
  const placePage = document.querySelector('.place-page');
  if (placePage) {
    placePage.remove();
  }
}

function addPlacePageStyles() {
  const style = document.createElement('style');
  style.textContent = `
    /* Place Page Container */
    .place-page {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: #1a1a1f;
      z-index: 2000;
      overflow-y: auto;
      padding: 20px;
      box-sizing: border-box;
      font-family: 'Inter', sans-serif;
    }

    /* Place Header */
    .place-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 20px;
      padding: 15px;
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      backdrop-filter: blur(10px);
    }

    .back-button {
      background: none;
      border: none;
      color: #fff;
      font-size: 1.5em;
      cursor: pointer;
      transition: transform 0.2s ease, opacity 0.2s ease;
    }

    .back-button:hover {
      transform: scale(1.1);
      opacity: 0.8;
    }

    .place-header h2 {
      margin: 0;
      font-size: 1.5em;
      color: #fff;
      font-weight: 600;
      letter-spacing: -0.5px;
    }

    /* Place Info */
    .place-info {
      text-align: center;
      margin-bottom: 20px;
    }

    .place-logo {
      width: 100px;
      height: 100px;
      border-radius: 50%;
      object-fit: cover;
      border: 3px solid rgba(255, 215, 0, 0.5);
      margin-bottom: 15px;
      transition: transform 0.3s ease;
    }

    .place-logo:hover {
      transform: scale(1.05);
    }

    .place-info h3 {
      margin: 0;
      font-size: 1.75em;
      color: #fff;
      font-weight: 600;
      letter-spacing: -0.5px;
    }

    .place-description {
      color: rgba(255, 255, 255, 0.8);
      font-size: 1em;
      margin-top: 10px;
      line-height: 1.5;
    }

    /* Social Media Icons */
    .social-media {
      display: flex;
      justify-content: center;
      gap: 20px;
      margin-top: 20px;
    }

    .social-media a {
      color: #fff;
      font-size: 1.75em;
      transition: transform 0.3s ease, color 0.3s ease;
    }

    .social-media a:hover {
      color: #ffd700;
      transform: scale(1.2);
    }

    /* Tabs */
    .place-tabs {
      display: flex;
      justify-content: space-around;
      margin-bottom: 20px;
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      padding: 10px;
      backdrop-filter: blur(10px);
    }

    .tab-button {
      background: none;
      border: none;
      color: rgba(255, 255, 255, 0.7);
      font-size: 1em;
      cursor: pointer;
      padding: 12px 24px;
      border-radius: 12px;
      transition: all 0.3s ease;
    }

    .tab-button.active {
      background: rgba(255, 215, 0, 0.1);
      color: #ffd700;
      font-weight: 600;
    }

    .tab-button:hover {
      background: rgba(255, 215, 0, 0.1);
    }

    /* Tab Content */
    .tab-content {
      padding-bottom: 200px;
    }

    .tab-pane {
      display: none;
      animation: fadeIn 0.5s ease;
    }

    .tab-pane.active {
      display: block;
    }

    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }

    /* Facilities List */
    .facilities-list {
      display: grid;
      gap: 20px;
    }

    .facility-item {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      padding: 20px;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      backdrop-filter: blur(10px);
    }

    .facility-item:hover {
      transform: translateY(-5px);
      box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
    }

    .facility-item h5 {
      margin: 0;
      font-size: 1.1em;
      color: #fff;
      font-weight: 600;
    }

    .facility-item p {
      margin: 10px 0;
      color: rgba(255, 255, 255, 0.8);
      font-size: 0.95em;
    }

    /* Events List */
    .events-list {
      display: grid;
      gap: 20px;
    }

    .event-item {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      padding: 20px;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      backdrop-filter: blur(10px);
    }

    .event-item:hover {
      transform: translateY(-5px);
      box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
    }

    .event-item h5 {
      margin: 0;
      font-size: 1.1em;
      color: #fff;
      font-weight: 600;
    }

    .event-item p {
      margin: 10px 0;
      color: rgba(255, 255, 255, 0.8);
      font-size: 0.95em;
    }

    /* Customer Support */
    .customer-support {
      text-align: center;
      padding: 20px;
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      backdrop-filter: blur(10px);
    }

    .customer-support p {
      margin: 10px 0;
      color: rgba(255, 255, 255, 0.8);
      font-size: 1em;
    }

    .customer-support button {
      background: rgba(255, 215, 0, 0.1);
      border: none;
      color: #fff;
      padding: 12px 24px;
      border-radius: 12px;
      cursor: pointer;
      transition: background 0.3s ease, transform 0.3s ease;
    }

    .customer-support button:hover {
      background: rgba(255, 215, 0, 0.2);
      transform: scale(1.05);
    }

    /* Chat Container */
    .chat-container {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      width: 100%;
      max-width: 100%;
      z-index: 1000;
      padding: 20px;
      background: #1a1a1f;
      border-top: 1px solid rgba(255, 215, 0, 0.1);
      display: flex;
      gap: 12px;
      align-items: center;
      box-sizing: border-box;
      backdrop-filter: blur(10px);
    }

    #placeInput {
      flex: 1;
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid rgba(255, 215, 0, 0.2);
      border-radius: 12px;
      padding: 20px;
      color: #fff;
      font-size: 1em;
      width: 100%;
      transition: border-color 0.3s ease;
    }

    #placeInput:focus {
      border-color: rgba(255, 215, 0, 0.5);
    }

    /* Responsive Design */
    @media (max-width: 768px) {
      .place-header h2 {
        font-size: 1.2em;
      }

      .place-logo {
        width: 80px;
        height: 80px;
      }

      .place-description {
        font-size: 0.9em;
      }

      .tab-button {
        font-size: 0.9em;
        padding: 10px 20px;
      }

      .facilities-list {
        grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
      }

      .facility-item h5 {
        font-size: 1em;
      }

      .facility-item p {
        font-size: 0.85em;
      }

      .event-item h5 {
        font-size: 1em;
      }

      .event-item p {
        font-size: 0.85em;
      }

      .customer-support button {
        font-size: 0.9em;
      }
    }

    /* Small Mobile Optimizations */
    @media (max-width: 480px) {
      .place-page {
        padding: 10px;
      }

      .facilities-list {
        grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
        gap: 10px;
      }

      .tab-button {
        padding: 8px 16px;
        font-size: 0.85em;
      }

      .chat-container {
        padding: 20px;
      }

      #placeInput {
        padding: 20px;
      }
    }
  `;
  document.head.appendChild(style);
}

// Helper function to escape HTML special characters
function escapeHTML(str) {
  const div = document.createElement('div');
  div.textContent = str;
  return div.innerHTML;
}

// Function to filter places based on category
function filterPlaces(category) {
  const placesGrid = document.getElementById('placesGrid');
  let filteredData = publicPlacesData;

  if (category !== 'All') {
    filteredData = filteredData.filter(place => place.category === category);
  }

  placesGrid.innerHTML = renderPlaceCards(filteredData);

  const filterButtons = document.querySelectorAll('.filter-button');
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === category) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

// Additional placeholder functions (you might want to implement these)
function selectPlace(category) {
  console.log(`Selected place in category: ${category}`);
}

function selectPlace(category) {
  // Optional: Add any specific action when a place card is clicked
  console.log(`Selected place in category: ${category}`);
}

const onlinePlatformsData = [
  // E-commerce
  {
    category: "E-commerce",
    details: {
      name: "Amazon",
      image: "https://example.com/amazon.jpg",
      description: "The world's largest online retailer offering a wide range of products.",
      website: "https://www.amazon.com",
      features: [
        { name: "Fast Delivery", description: "Get your orders delivered quickly" },
        { name: "Prime Membership", description: "Exclusive benefits for Prime members" }
      ],
      social: {
        facebook: "https://facebook.com/amazon",
        twitter: "https://twitter.com/amazon",
        instagram: "https://instagram.com/amazon"
      },
      support: {
        email: "support@amazon.com",
        phone: "+1-800-123-4567"
      }
    }
  },
  {
    category: "E-commerce",
    details: {
      name: "eBay",
      image: "https://example.com/ebay.jpg",
      description: "A global marketplace for buying and selling unique items.",
      website: "https://www.ebay.com",
      features: [
        { name: "Auction System", description: "Bid on items to get the best price" },
        { name: "Buy It Now", description: "Purchase items instantly" }
      ],
      social: {
        facebook: "https://facebook.com/ebay",
        twitter: "https://twitter.com/ebay",
        instagram: "https://instagram.com/ebay"
      },
      support: {
        email: "support@ebay.com",
        phone: "+1-800-987-6543"
      }
    }
  },
  // Streaming
  {
    category: "Streaming",
    details: {
      name: "Netflix",
      image: "https://example.com/netflix.jpg",
      description: "Stream movies, TV shows, and original content.",
      website: "https://www.netflix.com",
      features: [
        { name: "Offline Viewing", description: "Download content to watch offline" },
        { name: "Multiple Profiles", description: "Create profiles for family members" }
      ],
      social: {
        facebook: "https://facebook.com/netflix",
        twitter: "https://twitter.com/netflix",
        instagram: "https://instagram.com/netflix"
      },
      support: {
        email: "support@netflix.com",
        phone: "+1-800-555-1234"
      }
    }
  },
  {
    category: "Streaming",
    details: {
      name: "Spotify",
      image: "https://example.com/spotify.jpg",
      description: "Stream music and podcasts from around the world.",
      website: "https://www.spotify.com",
      features: [
        { name: "Playlists", description: "Create and share playlists" },
        { name: "Offline Mode", description: "Download music for offline listening" }
      ],
      social: {
        facebook: "https://facebook.com/spotify",
        twitter: "https://twitter.com/spotify",
        instagram: "https://instagram.com/spotify"
      },
      support: {
        email: "support@spotify.com",
        phone: "+1-800-555-9876"
      }
    }
  },
  // Social Media
  {
    category: "Social Media",
    details: {
      name: "Facebook",
      image: "https://example.com/facebook.jpg",
      description: "Connect with friends, family, and the world.",
      website: "https://www.facebook.com",
      features: [
        { name: "News Feed", description: "Stay updated with friends' posts" },
        { name: "Groups", description: "Join communities of interest" }
      ],
      social: {
        facebook: "https://facebook.com/facebook",
        twitter: "https://twitter.com/facebook",
        instagram: "https://instagram.com/facebook"
      },
      support: {
        email: "support@facebook.com",
        phone: "+1-800-555-4321"
      }
    }
  },
  {
    category: "Social Media",
    details: {
      name: "Instagram",
      image: "https://example.com/instagram.jpg",
      description: "Share photos and videos with your followers.",
      website: "https://www.instagram.com",
      features: [
        { name: "Stories", description: "Share moments that disappear after 24 hours" },
        { name: "Reels", description: "Create and watch short videos" }
      ],
      social: {
        facebook: "https://facebook.com/instagram",
        twitter: "https://twitter.com/instagram",
        instagram: "https://instagram.com/instagram"
      },
      support: {
        email: "support@instagram.com",
        phone: "+1-800-555-6789"
      }
    }
  }
];


// Function to show the platforms overlay
function showPlatformsOverlay() {
  let overlay = document.getElementById('platformsOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'platformsOverlay';
    overlay.className = 'platforms-overlay';
    document.body.appendChild(overlay);
  }

  const categories = ['All', ...new Set(onlinePlatformsData.map(platform => platform.category))];

  overlay.innerHTML = `
    <div class="platforms-content">
      <div class="platforms-header">
        <h2>Online Platforms</h2>
        <button class="close-platforms" onclick="hidePlatformsOverlay()">
          <i class="fas fa-long-arrow-alt-left"></i>
        </button>
      </div>
      
      <div class="platforms-filter-container">
        <div class="platforms-filter-buttons">
          ${categories.map(category => `
            <button 
              class="filter-button ${category === 'All' ? 'active' : ''}" 
              data-category="${category}" 
              onclick="filterPlatforms('${category}')"
            >
              ${category}
            </button>
          `).join('')}
        </div>
      </div>

      <div class="platforms-grid" id="platformsGrid">
        ${renderPlatformCards(onlinePlatformsData)}
      </div>
    </div>

    <div class="chat-container">
      <div class="input-container">
        <input type="text" id="platformsExploreInput" placeholder="Search for services or locations..." />
        <button class="voice-search" id="voiceSearchButton">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  const exploreInput = document.getElementById('platformsExploreInput');
  const platformsGrid = document.getElementById('platformsGrid');
  const filterButtons = document.querySelectorAll('.filter-button');

  const debouncedSearch = debounce((query) => {
    performSearch(onlinePlatformsData, query, platformsGrid, filterButtons, renderPlatformCards);
  }, 300);

  exploreInput.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
  });

  exploreInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
      performSearch(onlinePlatformsData, exploreInput.value, platformsGrid, filterButtons, renderPlatformCards);
    }
  });

  initializeOverlayVoiceSearch(exploreInput, (query) => {
    performSearch(onlinePlatformsData, query, platformsGrid, filterButtons, renderPlatformCards);
  });

  setTimeout(() => overlay.classList.add('active'), 10);

  const style = document.createElement('style');
  style.textContent = `
  .platforms-overlay {
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  bottom: 60px;
  background: #1a1a1f;
  z-index: 1000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  overflow-y: auto;
  width: 100%;
  display: flex;
  flex-direction: column;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.platforms-overlay::-webkit-scrollbar {
  display: none;
}

.platforms-overlay.active {
  opacity: 1;
  visibility: visible;
}

.platforms-content {
  flex: 1;
  width: 100%;
  max-width: 100%;
  padding: 20px;
  margin: 0 auto;
  position: relative;
}

.platforms-header {
  padding: 16px 24px;
  background: rgba(26, 26, 31, 0.95);
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 30;
  border-bottom: 1px solid rgba(255, 215, 0, 0.15);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  height: 72px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
}

.platforms-header h2 {
  color: var(--primary-color);
  margin: 0;
  font-size: 1.75em;
  font-weight: 600;
  letter-spacing: -0.02em;
  display: flex;
  align-items: center;
  gap: 12px;
}

.platforms-header h2::before {
  content: '';
  display: inline-block;
  width: 24px;
  height: 24px;
  background: var(--primary-color);
  mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpath d='M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z'%3E%3C/path%3E%3Ccircle cx='12' cy='10' r='3'%3E%3C/circle%3E%3C/svg%3E");
  mask-repeat: no-repeat;
  mask-position: center;
  mask-size: contain;
}

.header-buttons-container {
  display: flex;
  align-items: center;
  gap: 16px;
}

.close-platforms {
  background: rgba(255, 215, 0, 0.1);
  border: 1px solid rgba(255, 215, 0, 0.2);
  color: var(--primary-color);
  font-size: 1.25em;
  cursor: pointer;
  padding: 8px 16px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.close-platforms:hover {
  background: rgba(255, 215, 0, 0.15);
  transform: translateY(-1px);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.close-platforms:active {
  transform: translateY(0);
}

.platforms-filter-container {
  position: fixed;
  top: 70px;
  left: 0;
  width: 100vw;
  max-width: 100vw;
  z-index: 10;
  background: #1a1a1f;
  margin-left: 10px;
  padding: 10px 15px;
  box-sizing: border-box;
}

.platforms-filter-buttons {
  display: flex;
  justify-content: flex-start;
  gap: 4px;
  padding-bottom: 10px;
  width: 100vw;
  margin-left: calc(-50vw + 50%);
  overflow-x: auto;
  scrollbar-width: none;
}

.platforms-filter-buttons::-webkit-scrollbar {
  height: 0px;
}

.platforms-filter-buttons::-webkit-scrollbar-thumb {
  background-color: var(--primary-color);
  border-radius: 3px;
}

.filter-button {
  margin-top: 2px;
  margin-left: 2px;
  margin-right: 5px;
  flex-shrink: 0;
  background: rgba(255, 215, 0, 0.1);
  color: var(--text-light);
  border: 1px solid rgba(255, 215, 0, 0.2);
  padding: 10px 15px;
  border-radius: 25px;
  transition: all 0.3s ease;
  cursor: pointer;
  white-space: nowrap;
  font-weight: 500;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.filter-button:hover {
  background: var(--primary-color);
  color: black;
  transform: scale(1.05);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.filter-button.active {
  background: var(--primary-color);
  color: black;
  font-weight: 700;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
}

.platforms-grid-container {
  flex: 1;
  width: 100%;
  padding-bottom: 20px;
  position: relative;
}

.platforms-grid {
  margin-top: 70px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
  gap: 20px;
  animation: fadeInUp 0.5s ease-out;
  padding-bottom: 140px;
  width: 100%;
}

.platform-card {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 16px;
  overflow: hidden;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  cursor: pointer;
}

.platform-card:hover {
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  border-color: rgba(255, 215, 0, 0.3);
}

.platform-card-header {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 20px;
  background: rgba(255, 255, 255, 0.03);
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
}

.brand-logo {
  width: 60px;
  height: 60px;
  border-radius: 12px;
  object-fit: cover;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.platform-card-title {
  flex: 1;
}

.platform-card-title h3 {
  margin: 0;
  font-size: 1.2em;
  color: #fff;
  font-weight: 600;
}

.category-tag {
  margin: 5px 0 0;
  font-size: 0.9em;
  color: rgba(255, 215, 0, 0.8);
  font-weight: 500;
}

.platform-card-body {
  padding: 20px;
}

.platform-description {
  margin: 0;
  font-size: 0.95em;
  color: rgba(255, 255, 255, 0.8);
  line-height: 1.5;
}

.platform-card-actions {
  display: flex;
  gap: 10px;
  padding: 15px 20px;
  background: rgba(255, 255, 255, 0.03);
  border-top: 1px solid rgba(255, 255, 255, 0.05);
}

.action-button {
  flex: 1;
  background: rgba(255, 215, 0, 0.1);
  border: none;
  border-radius: 8px;
  padding: 10px;
  color: #fff;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-size: 0.9em;
}

.action-button:hover {
  background: rgba(255, 215, 0, 0.2);
  transform: translateY(-2px);
}

.action-button i {
  font-size: 1.1em;
}

/* Responsive Design */
@media (max-width: 768px) {
  .platform-card-header {
    padding: 15px;
  }

  .brand-logo {
    width: 50px;
    height: 50px;
  }

  .platform-card-title h3 {
    font-size: 1.1em;
  }

  .platform-card-body {
    padding: 15px;
  }

  .platform-card-actions {
    padding: 10px 15px;
    flex-wrap: wrap;
  }

  .action-button {
    flex: 1 1 45%;
    padding: 8px;
    font-size: 0.85em;
  }
}

  .chat-container {
    position: fixed;
    bottom: 60px;
    left: 0;
    right: 0;
    width: 100%;
    max-width: 100%;
    z-index: 1000;
    padding: 20px;
    background: #1a1a1f;
    border-top: 1px solid rgba(255, 255, 255, 0.1);
    display: flex;
    gap: 12px;
    align-items: center;
    box-sizing: border-box;
  }

  .input-container {
    flex: 1;
    position: relative;
    display: flex;
    align-items: center;
  }

  #platformsExploreInput {
    flex: 1;
    background: rgba(255, 255, 255, 0.05);
    border: 1px solid rgba(255, 215, 0, 0.2);
    border-radius: 12px;
    padding: 12px 50px 12px 20px;
    color: #fff;
    font-size: 0.95em;
    width: 100%;
    transition: border-color 0.3s ease;
  }

  #platformsExploreInput:focus {
    border-color: rgba(255, 255, 255, 0.5);
  }

  .voice-search {
    position: absolute;
    right: 10px;
    background: none;
    border: none;
    color: var(--primary-color);
    cursor: pointer;
    padding: 10px;
    transition: all 0.3s ease;
}

  .voice-search:hover {
    opacity: 0.8;
  }

  .voice-search.active {
    color: #ffd700;
    animation: pulse 1.5s infinite;
  }

  .voice-search i { 
    font-size: 1.5em; 
}

  @keyframes pulse {
    0% { transform: scale(1); }
    50% { transform: scale(1.1); }
    100% { transform: scale(1); }
  }

/* Desktop Specific Styles */
@media (min-width: 769px) {
  .chat-container {
    padding: 24px;
    bottom: 60px;
  }

  .input-container {
    max-width: 800px;
    margin: 0 auto;
  }

  #platformsExploreInput {
    height: 80px;
    font-size: 1.05rem;
    padding: 0 50px 0 24px;
    border-radius: 16px;
  }

  .voice-search {
    width: 50px;
    height: 50px;
    right: 8px;
  }

  .voice-search i {
    font-size: 2.1em;
  }
}

/* Tablet Specific Styles */
@media (min-width: 769px) and (max-width: 1024px) {
  .input-container {
    max-width: 700px;
  }
}

/* Desktop and laptop styles */
@media (min-width: 769px) {
  .chat-container {
    width: 100%;
    display: flex;
    justify-content: center;
  }
  
  #platformsExploreInput {
    width: 60%;
    max-width: 1200px;
  }
  
  .platforms-grid {
    padding-bottom: 180px;
  }
}

/* Mobile styles */
@media (max-width: 768px) {
  .chat-container {
    width: 100%;
    left: 0;
    transform: none;
    padding: 10px;
    gap: 8px;
  }

  .platforms-grid {
    grid-template-columns: 1fr;
    padding: 0 10px;
    margin-top: 80px;
    gap: 15px;
  }

  .platform-card {
    width: 100%;
    margin: 0;
  }

  .platforms-grid-container {
    width: 100%;
    max-width: 100%;
    padding: 0;
  }

  .platforms-content {
    padding: 10px 0;
    padding-bottom: 200px;
  }

  #platformsExploreInput {
    width: 100%;
    padding: 20px 20px;
  }
}
   `;
  document.head.appendChild(style);
}

function renderPlatformCards(filteredData) {
  return filteredData.map(platform => `
    <div class="platform-card" onclick="openPlatformPage('${platform.category}')">
      <div class="platform-card-header">
        <img src="${platform.details.image}" alt="${platform.details.name}" class="brand-logo">
        <div class="platform-card-title">
          <h3>${platform.details.name}</h3>
          <p class="category-tag">${platform.category}</p>
        </div>
      </div>
      <div class="platform-card-body">
        <p class="platform-description">${platform.details.description}</p>
      </div>
      <div class="platform-card-actions">
        <a href="${platform.details.website}" target="_blank" class="action-button" onclick="event.stopPropagation()">
          <i class="fas fa-globe"></i> Visit Website
        </a>
        <button class="action-button" onclick="openPlatformPage('${platform.category}'); event.stopPropagation()">
          <i class="fas fa-info-circle"></i> Info
        </button>
        <!-- Chat Button for Amazon -->
        <button class="action-button chat-button" onclick="showChatSupport('${platform.details.name}'); event.stopPropagation()">
          <i class="fas fa-comments"></i> Chat
        </button>
      </div>
    </div>
  `).join('');
}

function openPlatformPage(category) {
  const platform = onlinePlatformsData.find(pl => pl.category === category);
  if (!platform) return;

  const platformPage = document.createElement('div');
  platformPage.className = 'platform-page';
  platformPage.innerHTML = `
    <div class="platform-header">
      <button class="back-button" onclick="closePlatformPage()">
        <i class="fas fa-arrow-left"></i> Back
      </button>
      <h2>${platform.details.name}</h2>
    </div>
    <div class="platform-content">
      <!-- Platform Info -->
      <div class="platform-info">
        <img src="${platform.details.image}" alt="${platform.details.name}" class="platform-logo">
        <h3>${platform.details.name}</h3>
        <p class="platform-description">${platform.details.description}</p>
        <div class="social-media">
          <a href="${platform.details.social?.facebook}" target="_blank"><i class="fab fa-facebook"></i></a>
          <a href="${platform.details.social?.twitter}" target="_blank"><i class="fab fa-twitter"></i></a>
          <a href="${platform.details.social?.instagram}" target="_blank"><i class="fab fa-instagram"></i></a>
        </div>
      </div>
      
      <!-- Tabs -->
      <div class="platform-tabs">
        <button class="tab-button active" data-tab="info">Info</button>
        <button class="tab-button" data-tab="features">Features</button>
        <button class="tab-button" data-tab="support">Support</button>
      </div>
      
      <!-- Tab Content -->
      <div class="tab-content">
        <div id="info" class="tab-pane active">
          <!-- Basic Info -->
          <div class="basic-info">
            <h4>Website</h4>
            <p><a href="${platform.details.website}" target="_blank">${platform.details.website}</a></p>
          </div>
        </div>
        <div id="features" class="tab-pane">
          <!-- Features Content -->
          <div class="features-list">
            <h4>Features</h4>
            ${platform.details.features?.map(feature => `
              <div class="feature-item">
                <h5>${feature.name}</h5>
                <p>${feature.description}</p>
              </div>
            `).join('')}
          </div>
        </div>
        <div id="support" class="tab-pane">
          <!-- Customer Support Content -->
          <div class="customer-support" id="supportContent">
            <h4>Contact Support</h4>
            <p>Email: ${platform.details.support?.email}</p>
            <p>Phone: ${platform.details.support?.phone}</p>
            <button onclick="showChatSupport('${platform.category}')">
              <i class="fas fa-comments"></i> Chat with Us
            </button>
          </div>
        </div>
      </div>
      
      <!-- Search Container -->
      <div class="chat-container">
        <input type="text" id="platformInput" placeholder="Search for features or support..." />
        <div class="input-actions">
          <button class="send-message" id="platformButton">
            <i class="fas fa-paper-plane"></i>
          </button>
        </div>
      </div>
    </div>
  `;

  document.body.appendChild(platformPage);
  addPlatformPageStyles();
  initializeTabs();
  initializeSearch();
  scrollToTop();
}

// Function to close the platform page
function closePlatformPage() {
  const platformPage = document.querySelector('.platform-page');
  if (platformPage) {
    platformPage.remove();
  }
}

function addPlatformPageStyles() {
  const style = document.createElement('style');
  style.textContent = `
    /* Platform Page Container */
    .platform-page {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: #1a1a1f;
      z-index: 2000;
      overflow-y: auto;
      padding: 20px;
      box-sizing: border-box;
      font-family: 'Inter', sans-serif;
    }

    /* Platform Header */
    .platform-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 20px;
      padding: 15px;
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      backdrop-filter: blur(10px);
    }

    .back-button {
      background: none;
      border: none;
      color: #fff;
      font-size: 1.5em;
      cursor: pointer;
      transition: transform 0.2s ease, opacity 0.2s ease;
    }

    .back-button:hover {
      transform: scale(1.1);
      opacity: 0.8;
    }

    .platform-header h2 {
      margin: 0;
      font-size: 1.5em;
      color: #fff;
      font-weight: 600;
      letter-spacing: -0.5px;
    }

    /* Platform Info */
    .platform-info {
      text-align: center;
      margin-bottom: 20px;
    }

    .platform-logo {
      width: 100px;
      height: 100px;
      border-radius: 50%;
      object-fit: cover;
      border: 3px solid rgba(255, 215, 0, 0.5);
      margin-bottom: 15px;
      transition: transform 0.3s ease;
    }

    .platform-logo:hover {
      transform: scale(1.05);
    }

    .platform-info h3 {
      margin: 0;
      font-size: 1.75em;
      color: #fff;
      font-weight: 600;
      letter-spacing: -0.5px;
    }

    .platform-description {
      color: rgba(255, 255, 255, 0.8);
      font-size: 1em;
      margin-top: 10px;
      line-height: 1.5;
    }

    /* Social Media Icons */
    .social-media {
      display: flex;
      justify-content: center;
      gap: 20px;
      margin-top: 20px;
    }

    .social-media a {
      color: #fff;
      font-size: 1.75em;
      transition: transform 0.3s ease, color 0.3s ease;
    }

    .social-media a:hover {
      color: #ffd700;
      transform: scale(1.2);
    }

    /* Tabs */
    .platform-tabs {
      display: flex;
      justify-content: space-around;
      margin-bottom: 20px;
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      padding: 10px;
      backdrop-filter: blur(10px);
    }

    .tab-button {
      background: none;
      border: none;
      color: rgba(255, 255, 255, 0.7);
      font-size: 1em;
      cursor: pointer;
      padding: 12px 24px;
      border-radius: 12px;
      transition: all 0.3s ease;
    }

    .tab-button.active {
      background: rgba(255, 215, 0, 0.1);
      color: #ffd700;
      font-weight: 600;
    }

    .tab-button:hover {
      background: rgba(255, 215, 0, 0.1);
    }

    /* Tab Content */
    .tab-content {
      padding-bottom: 200px;
    }

    .tab-pane {
      display: none;
      animation: fadeIn 0.5s ease;
    }

    .tab-pane.active {
      display: block;
    }

    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }

    /* Features List */
    .features-list {
      display: grid;
      gap: 20px;
    }

    .feature-item {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      padding: 20px;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      backdrop-filter: blur(10px);
    }

    .feature-item:hover {
      transform: translateY(-5px);
      box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
    }

    .feature-item h5 {
      margin: 0;
      font-size: 1.1em;
      color: #fff;
      font-weight: 600;
    }

    .feature-item p {
      margin: 10px 0;
      color: rgba(255, 255, 255, 0.8);
      font-size: 0.95em;
    }

    /* Customer Support */
    .customer-support {
      text-align: center;
      padding: 20px;
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      backdrop-filter: blur(10px);
    }

    .customer-support p {
      margin: 10px 0;
      color: rgba(255, 255, 255, 0.8);
      font-size: 1em;
    }

    .customer-support button {
      background: rgba(255, 215, 0, 0.1);
      border: none;
      color: #fff;
      padding: 12px 24px;
      border-radius: 12px;
      cursor: pointer;
      transition: background 0.3s ease, transform 0.3s ease;
    }

    .customer-support button:hover {
      background: rgba(255, 215, 0, 0.2);
      transform: scale(1.05);
    }

    /* Chat Container */
    .chat-container {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      width: 100%;
      max-width: 100%;
      z-index: 1000;
      padding: 20px;
      background: #1a1a1f;
      border-top: 1px solid rgba(255, 215, 0, 0.1);
      display: flex;
      gap: 12px;
      align-items: center;
      box-sizing: border-box;
      backdrop-filter: blur(10px);
    }

    #platformInput {
      flex: 1;
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid rgba(255, 215, 0, 0.2);
      border-radius: 12px;
      padding: 20px;
      color: #fff;
      font-size: 1em;
      width: 100%;
      transition: border-color 0.3s ease;
    }

    #platformInput:focus {
      border-color: rgba(255, 215, 0, 0.5);
    }

    /* Responsive Design */
    @media (max-width: 768px) {
      .platform-header h2 {
        font-size: 1.2em;
      }

      .platform-logo {
        width: 80px;
        height: 80px;
      }

      .platform-description {
        font-size: 0.9em;
      }

      .tab-button {
        font-size: 0.9em;
        padding: 10px 20px;
      }

      .features-list {
        grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
      }

      .feature-item h5 {
        font-size: 1em;
      }

      .feature-item p {
        font-size: 0.85em;
      }

      .customer-support button {
        font-size: 0.9em;
      }
    }

    /* Small Mobile Optimizations */
    @media (max-width: 480px) {
      .platform-page {
        padding: 10px;
      }

      .features-list {
        grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
        gap: 10px;
      }

      .tab-button {
        padding: 8px 16px;
        font-size: 0.85em;
      }

      .chat-container {
        padding: 20px;
      }

      #platformInput {
        padding: 20px;
      }
    }
  `;
  document.head.appendChild(style);
}

function filterPlatforms(category, filterType) {
  const platformsGrid = document.getElementById('platformsGrid');

  let filteredData = onlinePlatformsData;

  // Filter by selected category if not 'All'
  if (category !== 'All') {
    filteredData = filteredData.filter(platform => platform.category === category);
  }

  // Update the grid with filtered results
  platformsGrid.innerHTML = renderPlatformCards(filteredData);

  // Update active state of filter buttons
  const filterButtons = document.querySelectorAll('.filter-button');
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === category) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

// Additional placeholder functions (you might want to implement these)
function selectPlatform(category) {
  console.log(`Selected platform in category: ${category}`);
}

function selectPlatform(category) {
  // Optional: Add any specific action when a platform card is clicked
  console.log(`Selected platform in category: ${category}`);
}

// Modified hide functions for each overlay
function hideLocatorsOverlay() {
  const overlay = document.getElementById('locatorsOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => {
      overlay.remove();
      overlayState.currentOverlay = null;
    }, 300);
  }
}

function hideProvidersOverlay() {
  const overlay = document.getElementById('providersOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => {
      overlay.remove();
      overlayState.currentOverlay = null;
    }, 300);
  }
}

function hidePlacesOverlay() {
  const overlay = document.getElementById('placesOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => {
      overlay.remove();
      overlayState.currentOverlay = null;
    }, 300);
  }
}

function hidePlatformsOverlay() {
  const overlay = document.getElementById('platformsOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => {
      overlay.remove();
      overlayState.currentOverlay = null;
    }, 300);
  }
}


// First, add the voice search functionality
function initializeVoiceSearch(detailName) {
  if ('webkitSpeechRecognition' in window || 'SpeechRecognition' in window) {
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    const recognition = new SpeechRecognition();
    
    recognition.continuous = false;
    recognition.interimResults = false;
    recognition.lang = 'en-US';

    recognition.onresult = (event) => {
      const voiceQuery = event.results[0][0].transcript;
      const searchInput = document.getElementById('chatSearchInput');
      searchInput.value = voiceQuery;
      searchFAQs(voiceQuery, detailName);
    };

    recognition.onerror = (event) => {
      console.error('Speech recognition error:', event.error);
      showVoiceSearchError(event.error);
    };

    return recognition;
  }
  return null;
}

function showVoiceSearchError(error) {
  const searchResults = document.getElementById('searchResults');
  searchResults.innerHTML = `
    <div class="voice-search-error">
      <i class="fas fa-microphone-slash"></i>
      <h3>Voice Search Error</h3>
      <p>${getVoiceErrorMessage(error)}</p>
    </div>
  `;
}

function getVoiceErrorMessage(error) {
  switch (error) {
    case 'not-allowed':
      return 'Microphone access was denied. Please enable microphone access to use voice search.';
    case 'no-speech':
      return 'No speech was detected. Please try again.';
    case 'network':
      return 'Network error occurred. Please check your connection and try again.';
    default:
      return 'An error occurred with voice search. Please try again.';
  }
}

function showChatSupport(detailName) {
  const chatPage = document.createElement('div');
  chatPage.className = 'chat-support-page';
  chatPage.innerHTML = `
    <div class="chat-support-header">
      <button class="back-button" onclick="hideChatSupport()">
        <i class="fas fa-arrow-left"></i>
      </button>
      <h2>FAQ Chat Support - ${detailName}</h2>
    </div>
    <div class="chat-support-content">
      <div class="faq-section">
        <h3>Frequently Asked Questions</h3>
        <div class="faq-list" id="faqList">
          ${renderFAQsByDetail(detailName)}
        </div>
      </div>
      <div class="contact-support" id="contactSupport">
        <div class="contact-support-header">
          <h3>Need More Help?</h3>
          <button class="refresh-button" onclick="refreshChatSupport('${detailName}')">
            <i class="fas fa-sync-alt"></i>
          </button>
        </div>
        <a href="mailto:support@example.com" class="contact-button">
          <i class="fas fa-envelope"></i> Email Support
        </a>
        <a href="tel:+1234567890" class="contact-button">
          <i class="fas fa-phone"></i> Call Support
        </a>
      </div>
      <div class="search-results" id="searchResults"></div>
    </div>
    <div class="chat-search-container">
      <div class="input-container">
        <input type="text" id="chatSearchInput" placeholder="Search for answers..." />
        <button class="voice-search" id="voiceSearchBtn">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  document.body.appendChild(chatPage);
  addChatSupportStyles();
  initializeFAQToggle();
  initializeChatSearch(detailName);
  initializeVoiceSearchButton(detailName);
}

function initializeVoiceSearchButton(detailName) {
  const voiceSearchBtn = document.getElementById('voiceSearchBtn');
  const recognition = initializeVoiceSearch(detailName);
  
  if (recognition) {
    let isListening = false;

    voiceSearchBtn.addEventListener('click', () => {
      if (!isListening) {
        recognition.start();
        voiceSearchBtn.classList.add('listening');
        voiceSearchBtn.querySelector('i').classList.remove('fa-microphone');
        voiceSearchBtn.querySelector('i').classList.add('fa-microphone-slash');
      } else {
        recognition.stop();
        voiceSearchBtn.classList.remove('listening');
        voiceSearchBtn.querySelector('i').classList.remove('fa-microphone-slash');
        voiceSearchBtn.querySelector('i').classList.add('fa-microphone');
      }
      isListening = !isListening;
    });

    recognition.onend = () => {
      voiceSearchBtn.classList.remove('listening');
      voiceSearchBtn.querySelector('i').classList.remove('fa-microphone-slash');
      voiceSearchBtn.querySelector('i').classList.add('fa-microphone');
      isListening = false;
    };
  } else {
    voiceSearchBtn.style.display = 'none';
  }
}

function renderFAQsByDetail(detailName) {
  const faqs = getFAQsByDetail(detailName);
  return faqs.map(faq => `
    <div class="faq-item" onclick="toggleFAQAnswer('${faq.id}')">
      <div class="faq-question">
        <span>${faq.question}</span>
        <i class="fas fa-chevron-down"></i>
      </div>
      <div class="faq-answer" id="faqAnswer${faq.id}">
        <p>${faq.answer}</p>
      </div>
    </div>
  `).join('');
}

function getFAQsByDetail(detailName) {
  // Example FAQs, you can replace this with your own data
  const faqs = {
    "Dmart": [
      { id: 1, question: "What are the store timings for Dmart?", answer: "Dmart stores are open from 9 AM to 10 PM, Monday to Sunday." },
      { id: 2, question: "Does Dmart offer home delivery?", answer: "Yes, Dmart offers home delivery through their app and website." }
    ],
    "Amazon": [
      { id: 3, question: "How do I track my Amazon order?", answer: "You can track your order by logging into your Amazon account and visiting the 'Your Orders' section." },
      { id: 4, question: "What is Amazon Prime?", answer: "Amazon Prime is a subscription service that offers benefits like free delivery, access to streaming services, and more." }
    ],
    "Delhi Plumbing Services": [
      { id: 5, question: "What services do you offer?", answer: "We offer a wide range of plumbing services including pipe repair, leak fixing, and more." },
      { id: 6, question: "Are your plumbers certified?", answer: "Yes, all our plumbers are certified and experienced professionals." }
    ],
    "Lodhi Garden": [
      { id: 7, question: "What are the entry fees for Lodhi Garden?", answer: "Entry to Lodhi Garden is free for all visitors." },
      { id: 8, question: "Are pets allowed in Lodhi Garden?", answer: "Yes, pets are allowed in Lodhi Garden but must be kept on a leash." }
    ]
  };

  return faqs[detailName] || [];
}

function toggleFAQ(index) {
  const answer = document.getElementById(`faq-answer-${index}`);
  const question = answer.previousElementSibling;
  const chevron = question.querySelector('i');

  if (answer.classList.contains('active')) {
    // Collapse the answer
    answer.style.maxHeight = `${answer.scrollHeight}px`;
    setTimeout(() => {
      answer.style.maxHeight = '0';
    }, 10);
  } else {
    // Expand the answer
    answer.style.maxHeight = `${answer.scrollHeight}px`;
  }

  // Toggle the active class after a slight delay
  setTimeout(() => {
    answer.classList.toggle('active');
    question.classList.toggle('active');
    chevron.classList.toggle('fa-chevron-down');
    chevron.classList.toggle('fa-chevron-up');
  }, 10);
}

// Function to initialize FAQ toggle functionality
function initializeFAQToggle() {
  const faqQuestions = document.querySelectorAll('.faq-question');
  faqQuestions.forEach(question => {
    question.addEventListener('click', () => {
      const answer = question.nextElementSibling;
      answer.classList.toggle('active');
      question.classList.toggle('active');
    });
  });
}

// Function to initialize chat search functionality
function initializeChatSearch() {
  const chatSearchInput = document.getElementById('chatSearchInput');
  if (chatSearchInput) {
    chatSearchInput.addEventListener('input', (e) => {
      const query = e.target.value.trim();
      if (query) {
        searchFAQs(query);
      } else {
        clearSearchResults();
      }
    });
  }
}

// Function to search FAQs
function searchFAQs(query) {
  const faqList = document.getElementById('faqList');
  const searchResults = document.getElementById('searchResults');
  if (faqList && searchResults) {
    const faqItems = faqList.querySelectorAll('.faq-item');
    let resultsFound = false;

    faqItems.forEach(item => {
      const question = item.querySelector('.faq-question span').textContent.toLowerCase();
      const answer = item.querySelector('.faq-answer p').textContent.toLowerCase();
      if (question.includes(query.toLowerCase()) || answer.includes(query.toLowerCase())) {
        const resultItem = document.createElement('div');
        resultItem.className = 'search-result-item';
        resultItem.innerHTML = `
          <h4>${item.querySelector('.faq-question span').textContent}</h4>
          <p>${item.querySelector('.faq-answer p').textContent}</p>
        `;
        searchResults.appendChild(resultItem);
        resultsFound = true;
      }
    });

    if (!resultsFound) {
      searchResults.innerHTML = `<p>No results found for "${query}".</p>`;
    }
  }
}

// Function to clear search results
function clearSearchResults() {
  const searchResults = document.getElementById('searchResults');
  if (searchResults) {
    searchResults.innerHTML = '';
  }
}

// Function to hide chat support
function hideChatSupport() {
  const chatPage = document.querySelector('.chat-support-page');
  if (chatPage) {
    chatPage.remove();
  }
}

function refreshChatSupport(detailName) {
  const faqList = document.getElementById('faqList');
  if (faqList) {
    faqList.innerHTML = renderFAQsByDetail(detailName);
  }
}

// Function to add chat support styles
function addChatSupportStyles() {
  const style = document.createElement('style');
  style.textContent = `
    .chat-support-page {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: #1a1a1f;
      z-index: 2000;
      overflow-y: auto;
      animation: slideIn 0.3s ease-out;
    }

    @keyframes slideIn {
      from { transform: translateX(100%); }
      to { transform: translateX(0); }
    }

    .chat-support-header {
      position: sticky;
      top: 0;
      background: rgba(26, 26, 31, 0.95);
      font-size: 11px;
      padding: 20px;
      display: flex;
      align-items: center;
      gap: 20px;
      border-bottom: 2px solid var(--primary-color);
      backdrop-filter: blur(10px);
      z-index: 10;
    }

    .back-button {
      background: none;
      border: none;
      color: var(--primary-color);
      font-size: 1.2em;
      cursor: pointer;
      padding: 10px;
      border-radius: 50%;
      transition: all 0.3s ease;
    }

    .back-button:hover {
      background: rgba(255, 215, 0, 0.1);
    }

    .chat-support-content {
      max-width: 800px;
      margin: 0 auto;
      padding: 20px;
    }

    .faq-section {
      margin-top: 0px;
      transition: all 0.3s ease;
    }

    .faq-section h3 {
  color: #fff;
  margin-bottom: 14px;
  font-size: 20px;
  padding-left: 0px;
}

    .faq-item {
      background: rgba(255, 215, 0, 0.05);
      border-radius: 10px;
      margin-top: 5px;
      overflow: hidden;
      transition: all 0.3s ease;
    }

    .faq-question {
      padding: 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      cursor: pointer;
      color: var(--text-light);
      font-weight: 500;
      transition: all 0.3s ease;
    }

    .faq-question:hover {
      background: rgba(255, 215, 0, 0.1);
    }

    .faq-question i {
      transition: transform 0.3s ease;
    }

    .faq-question.active i {
      transform: rotate(180deg);
    }

    .faq-answer {
      padding: 0 20px;
      max-height: 0;
      overflow: hidden;
      transition: all 0.3s ease;
      color: #aaa;
    }

    .faq-answer.active {
      padding: 20px;
      max-height: 500px;
      border-top: 1px solid rgba(255, 215, 0, 0.1);
    }

  
    .contact-support {
      display: none;
      animation: fadeIn 0.3s ease-out;
      padding: 40px 20px;
      background: rgba(255, 215, 0, 0.05);
      border-radius: 15px;
      text-align: center;
      margin: 20px auto;
      max-width: 600px;
    }

    .contact-support-header {
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 15px;
      margin-bottom: 20px;
    }

    .refresh-button {
      background: none;
      border: none;
      color: var(--primary-color);
      font-size: 1.2em;
      cursor: pointer;
      padding: 8px;
      border-radius: 50%;
      transition: all 0.3s ease;
    }

    .refresh-button:hover {
      background: rgba(255, 215, 0, 0.1);
      transform: rotate(180deg);
    }

    .contact-button {
      display: flex;
      align-items: center;
      gap: 10px;
      padding: 15px 25px;
      background: rgba(255, 215, 0, 0.1);
      border-radius: 10px;
      color: var(--primary-color);
      text-decoration: none;
      transition: all 0.3s ease;
      margin: 10px 0;
    }

    .contact-button:hover {
      background: rgba(255, 215, 0, 0.2);
      transform: translateX(5px);
    }

    .chat-search-container {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      width: 100%;
      max-width: 100%;
      z-index: 1000;
      padding: 20px;
      background: #1a1a1f;
      border-top: 1px solid rgba(255, 215, 0, 0.1);
      display: flex;
      justify-content: center;
      align-items: center;
      box-sizing: border-box;
    }

    .input-container {
      position: relative;
      width: 100%;
      max-width: 800px;
      display: flex;
      align-items: center;
    }

    #chatSearchInput {
      width: 100%;
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid rgba(255, 215, 0, 0.2);
      border-radius: 12px;
      padding: 12px 50px 12px 20px;
      color: #fff;
      font-size: 0.95em;
      height: 60px;
      transition: border-color 0.3s ease;
    }

    #chatSearchInput:focus {
      border-color: rgba(255, 255, 255, 0.5);
      outline: none;
    }

    .voice-search {
      position: absolute;
      right: 10px;
      background: none;
      border: none;
      color: var(--primary-color);
      cursor: pointer;
      padding: 10px;
      transition: all 0.3s ease;
    }

    .voice-search:hover {
      opacity: 0.8;
    }

    .voice-search.active {
      color: #ffd700;
      animation: pulse 1.5s infinite;
    }

    .voice-search i {
      font-size: 1.5em;
    }

    @keyframes pulse {
      0% { transform: scale(1); }
      50% { transform: scale(1.1); }
      100% { transform: scale(1); }
    }

    .voice-search-error {
    text-align: center;
    padding: 40px 20px;
    color: var(--text-light);
  }

  .voice-search-error i {
    font-size: 3em;
    color: #ff4444;
    margin-bottom: 20px;
  }

  /* Responsive styles for the new button */
  @media (max-width: 768px) {
    .voice-search-btn {
      padding: 20px 20px;
    }
  }


    /* Desktop Specific Styles */
    @media (min-width: 769px) {
      .chat-search-container {
        padding: 24px;
        bottom: 0;
      }

      .input-container {
        max-width: 800px;
        margin: 0 auto;
      }

      #chatSearchInput {
        height: 80px;
        font-size: 1.05rem;
        padding: 0 50px 0 24px;
        border-radius: 16px;
      }

      .voice-search {
        width: 50px;
        height: 50px;
        right: 15px;
      }

      .voice-search i {
        font-size: 2.1em;
      }
    }

    /* Mobile styles */
    @media (max-width: 768px) {
    .chat-search-container {
    width: 100%;
    left: 0;
    transform: none;
    padding: 10px;
    gap: 8px;
  }
      #chatSearchInput {
    width: 100%;
    padding: 20px 20px;
  }
}

      .voice-search {
        width: 40px;
        height: 40px;
        right: 5px;
      }

      .voice-search i {
        font-size: 1.3em;
      }
    }

  `;
  document.head.appendChild(style);
}

function searchFAQs(query, detailName) {
  const faqList = document.getElementById('faqList');
  const searchResults = document.getElementById('searchResults');
  const contactSupport = document.getElementById('contactSupport');
  
  if (!query.trim()) {
    clearSearchResults();
    faqList.style.display = 'block';
    contactSupport.style.display = 'none';
    return;
  }

  if (faqList && searchResults) {
    const faqItems = faqList.querySelectorAll('.faq-item');
    const results = [];
    
    faqItems.forEach(item => {
      const question = item.querySelector('.faq-question span').textContent;
      const answer = item.querySelector('.faq-answer p').textContent;
      
      // Calculate relevance score using fuzzy matching
      const questionScore = calculateRelevance(query, question);
      const answerScore = calculateRelevance(query, answer);
      const totalScore = Math.max(questionScore, answerScore);

      if (totalScore > 0.3) { // Threshold for relevance
        results.push({
          question,
          answer,
          score: totalScore,
          element: item.cloneNode(true)
        });
      }
    });

    // Sort results by relevance score
    results.sort((a, b) => b.score - a.score);
    
    // Clear previous results
    searchResults.innerHTML = '';
    faqList.style.display = 'none';
    
    if (results.length > 0) {
      // Add search summary
      const summary = document.createElement('div');
      summary.className = 'search-summary';
      summary.innerHTML = `Found ${results.length} result${results.length === 1 ? '' : 's'} for "${query}"`;
      searchResults.appendChild(summary);

      // Display results with highlighted matches
      results.forEach(result => {
        const resultItem = document.createElement('div');
        resultItem.className = 'search-result-item';
        resultItem.innerHTML = `
          <h4>${highlightMatches(result.question, query)}</h4>
          <p>${highlightMatches(result.answer, query)}</p>
          <div class="relevance-indicator" style="width: ${result.score * 100}%"></div>
        `;
        searchResults.appendChild(resultItem);
      });
    } else {
      // Show "no results" message with suggestions
      searchResults.innerHTML = `
        <div class="no-results">
          <i class="fas fa-search"></i>
          <h3>No results found for "${query}"</h3>
          <p>Try:</p>
          <ul>
            <li>Checking your spelling</li>
            <li>Using more general terms</li>
            <li>Using fewer keywords</li>
          </ul>
          <button onclick="showContactSupport()" class="contact-support-btn">
            Contact Support
          </button>
        </div>
      `;
    }

    // Show/hide contact support based on results
    contactSupport.style.display = results.length === 0 ? 'block' : 'none';
  }
}

// Calculate relevance score using Levenshtein distance and token matching
function calculateRelevance(query, text) {
  const queryTokens = query.toLowerCase().split(/\s+/);
  const textTokens = text.toLowerCase().split(/\s+/);
  
  let totalScore = 0;
  queryTokens.forEach(queryToken => {
    let bestTokenScore = 0;
    textTokens.forEach(textToken => {
      const distance = levenshteinDistance(queryToken, textToken);
      const maxLength = Math.max(queryToken.length, textToken.length);
      const similarity = 1 - (distance / maxLength);
      bestTokenScore = Math.max(bestTokenScore, similarity);
    });
    totalScore += bestTokenScore;
  });
  
  return totalScore / queryTokens.length;
}

// Levenshtein distance calculation for fuzzy matching
function levenshteinDistance(str1, str2) {
  const matrix = Array(str2.length + 1).fill().map(() => Array(str1.length + 1).fill(0));
  
  for (let i = 0; i <= str1.length; i++) matrix[0][i] = i;
  for (let j = 0; j <= str2.length; j++) matrix[j][0] = j;
  
  for (let j = 1; j <= str2.length; j++) {
    for (let i = 1; i <= str1.length; i++) {
      const cost = str1[i - 1] === str2[j - 1] ? 0 : 1;
      matrix[j][i] = Math.min(
        matrix[j - 1][i] + 1,
        matrix[j][i - 1] + 1,
        matrix[j - 1][i - 1] + cost
      );
    }
  }
  
  return matrix[str2.length][str1.length];
}

// Highlight matching text in results
function highlightMatches(text, query) {
  const queryTokens = query.toLowerCase().split(/\s+/);
  let highlightedText = text;
  
  queryTokens.forEach(token => {
    const regex = new RegExp(`(${token})`, 'gi');
    highlightedText = highlightedText.replace(regex, '<mark>$1</mark>');
  });
  
  return highlightedText;
}

// Debounce search input to improve performance
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}

function initializeChatSearch(detailName) {
  const chatSearchInput = document.getElementById('chatSearchInput');
  if (chatSearchInput) {
    const debouncedSearch = debounce((e) => {
      const query = e.target.value.trim();
      searchFAQs(query, detailName);
    }, 300);
    
    chatSearchInput.addEventListener('input', debouncedSearch);
    
    // Add keyboard navigation
    chatSearchInput.addEventListener('keydown', (e) => {
      if (e.key === 'Escape') {
        clearSearchResults();
        chatSearchInput.value = '';
      }
    });
  }
}

// Additional styles for enhanced features
const additionalStyles = `
  .search-summary {
    color: var(--text-light);
    padding: 10px 0;
    margin-bottom: 15px;
    font-size: 0.9em;
  }

  .search-result-item {
    position: relative;
    overflow: hidden;
  }

  .search-result-item mark {
    background: rgba(255, 215, 0, 0.2);
    color: var(--primary-color);
    padding: 0 2px;
    border-radius: 2px;
  }

  .relevance-indicator {
    position: absolute;
    bottom: 0;
    left: 0;
    height: 2px;
    background: var(--primary-color);
    opacity: 0.5;
    transition: width 0.3s ease;
  }

  .no-results {
    text-align: center;
    padding: 40px 20px;
    color: var(--text-light);
  }

  .no-results i {
    font-size: 3em;
    color: var(--primary-color);
    margin-bottom: 20px;
  }

  .no-results ul {
    list-style: none;
    padding: 0;
    margin: 20px 0;
  }

  .no-results li {
    margin: 10px 0;
    color: #aaa;
  }

  .contact-support-btn {
    background: var(--primary-color);
    color: #1a1a1f;
    border: none;
    padding: 12px 24px;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 500;
    transition: all 0.3s ease;
  }

  .contact-support-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(255, 215, 0, 0.2);
  }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .search-result-item {
    animation: fadeIn 0.3s ease-out forwards;
    animation-delay: calc(var(--index) * 0.05s);
  }
`;

// Helper function to hide current overlay
function hideCurrentOverlay() {
  switch (overlayState.currentOverlay) {
    case 'locators':
      hideLocatorsOverlay();
      break;
    case 'providers':
      hideProvidersOverlay();
      break;
    case 'places':
      hidePlacesOverlay();
      break;
    case 'platforms':
      hidePlatformsOverlay();
      break;
  }
}

// Initialize the system when the DOM loads
document.addEventListener('DOMContentLoaded', initializeOverlaySystem);

function submitBookingViaWhatsApp() { const name = document.getElementById('nameInput').value.trim(); const contact = document.getElementById('contactInput').value.trim(); const address = document.getElementById('addressInput').value.trim(); const pincode = document.getElementById('pincodeInput').value.trim(); const date = document.getElementById('dateInput').value.trim(); const time = document.getElementById('timeInput').value.trim(); const bookingForm = document.getElementById('bookingForm'); const categoryId = bookingForm.dataset.categoryId || 'Not Specified'; const serviceName = bookingForm.dataset.serviceName || 'Not Specified'; const packageType = bookingForm.dataset.packageType || 'Not Specified'; if (!name || !contact) { showAlert('Please fill in Name and Contact Number', 'error'); return; } const message = `New Booking Request:\n📋 Category: ${categoryId}\n🛠 Service: ${serviceName}\n💎 Package: ${packageType}\n👤 Name: ${name}\n📞 Contact: ${contact}\n🏠 Address: ${address || 'Not Provided'}\n📍 Pincode: ${pincode || 'Not Provided'}\n📅 Date: ${date}\n⏰ Time: ${time}\n\nPlease confirm and process this booking.`; const encodedMessage = encodeURIComponent(message); const whatsappNumber = '78698 09022'; const whatsappUrl = `https://wa.me/${whatsappNumber.replace(/\s/g, '')}?text=${encodedMessage}`; window.open(whatsappUrl, '_blank'); showBookingConfirmation(); } function showBookingConfirmation() { const confirmationModal = document.createElement('div'); confirmationModal.id = 'bookingConfirmation'; confirmationModal.className = 'modal-overlay'; confirmationModal.innerHTML = `<div class="modal-content"><div class="confirmation-icon"><i class="fas fa-check-circle"></i></div><h2>Booking Submitted!</h2><p>Your booking request has been sent to WhatsApp. Our team will contact you shortly.</p><button onclick="closeConfirmation()" class="action-button">Close</button></div>`; document.body.appendChild(confirmationModal); document.getElementById('bookingForm').reset(); } function closeConfirmation() { const confirmationModal = document.getElementById('bookingConfirmation'); if (confirmationModal) { confirmationModal.remove(); } } document.addEventListener('DOMContentLoaded', function() { const submitBookingButton = document.getElementById('submitBooking'); if (submitBookingButton) { submitBookingButton.addEventListener('click', submitBookingViaWhatsApp); } }); document.addEventListener('DOMContentLoaded', function() { const locationNav = document.querySelector('.nav-item[data-page="location"]'); if (locationNav) { locationNav.addEventListener('click', function(e) { e.preventDefault(); showCitiesOverlay(); }); } }); function showAlert(message, type = 'success') { const alert = document.createElement('div'); alert.className = `alert alert-${type}`; alert.textContent = message; document.body.appendChild(alert); setTimeout(() => { alert.remove(); }, 3000); } function setupEventListeners() { window.addEventListener('click', function(event) { if (event.target.classList.contains('modal-overlay')) { closePackageInfo(); closeBookingForm(); closeConfirmation(); } }); }
                            </script>
                            
