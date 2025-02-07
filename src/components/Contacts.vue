<template>
  <footer class="footer">
    <div class="footer-container">
      <div class="contact-info">
        <h2>Контакты</h2>
        <p><strong>Телефон:</strong> <a href="tel:+79800376065">+7(900)123-45-67</a></p>
        <p><strong>Email:</strong> <a href="mailto:example@example.com">example@example.com</a></p>
      </div>
      <div class="social-links">
        <h2>Мы в соцсетях</h2>
        <div class="icons">
          <a href="#" class="icon" title="Telegram">
            <img src="./icons/telegram.png" alt="Telegram" />
          </a>
          <a href="#" class="icon" title="WhatsApp">
            <img src="./icons/wa.png" alt="WhatsApp" />
          </a>
        </div>
      </div>
    </div>
  </footer>
</template>

<style scoped>
body {
  margin: 0;
  font-family: 'Arial', sans-serif;
}
.footer {
  background-color: #2f393d;
  color: #efebe2;
  padding: 30px 20px;
  text-align: center;
}
.footer-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  max-width: 60%;
  margin: 0 auto;
}
@media (min-width: 768px) {
  .footer-container {
    flex-direction: row;
  }
}

.contact-info {
  flex: 1;
  margin-right: 20px;
  text-align: left;
}
.contact-info h2 {
  font-size: 22px;
  margin-bottom: 10px;
}
.contact-info p {
  margin: 5px 0;
}
.contact-info a {
  color: #ff8c42;
  text-decoration: none;
}
.contact-info a:hover {
  text-decoration: underline;
}

.social-links {
  flex: 1;
  margin-left: 20px;
  text-align: left;
}
.social-links h2 {
  font-size: 22px;
  margin-bottom: 10px;
}
.icons {
  display: flex;
  gap: 15px;
  margin-top: 10px;
}
.icon {
  font-size: 28px;
  text-decoration: none;
  display: flex;
  align-items: center;
  transition: transform 0.3s ease;
}
.icon img {
  height: 40px;
  width: 40px;
}
.icon:hover {
  transform: scale(1.2);
}

/* Копирайт */
.copyright {
  flex: 1;
  margin-top: 20px;
  text-align: center;
  font-size: 14px;
}
@media (min-width: 768px) {
  .copyright {
    margin-top: 0;
    text-align: right;
  }
}

@media (max-width: 768px) {
  .footer-container {
    flex-direction: column;
    align-items: center;
  }
  .contact-info,
  .social-links {
    text-align: center;
    font-size: 18px;
    margin: 10px 0;
  }
  .icons {
    justify-content: center;
  }
  .icon img {
    height: 60px;
    width: 60px;
  }
}
</style>
