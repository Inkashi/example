<template>
  <footer class="footer">
    <div class="footer-container">
      <div class="contact-info">
        <h2>Контакты</h2>
        <p><strong>Телефон:</strong> <a href="tel:+88005553535">+7 (900) 123-45-67</a></p>
        <p><strong>Email:</strong> <a href="mailto:info@example.com">info@example.com</a></p>
      </div>
      <div class="social-links">
        <h2>Мы в соцсетях</h2>
        <div class="icons">
          <a href="#" class="icon"><img src="./icons/telegram.png" />Telegram</a>
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
  justify-content: space-around;
  flex-wrap: wrap;
  max-width: 900px;
  margin: 0 auto;
}

.contact-info h2,
.social-links h2 {
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

.social-links .icons {
  display: flex;
  gap: 15px;
  justify-content: center;
  margin-top: 10px;
}

.icon {
  font-size: 28px;
  text-decoration: none;
  display: flex;
  align-items: center;
}

.icon {
  color: #ff8c42;
}

img {
  height: 50px;
  width: 50px;
  margin-right: 10px;
}

@media (max-width: 1024px) {
  .footer-container {
    padding: 0 10px;
  }

  .contact-info h2,
  .social-links h2 {
    font-size: 20px;
  }

  .contact-info p {
    font-size: 16px;
  }

  img {
    height: 40px;
    width: 40px;
  }
}

@media (max-width: 768px) {
  .footer-container {
    padding: 0 10px;
  }

  .contact-info h2,
  .social-links h2 {
    font-size: 18px;
  }

  .contact-info p {
    font-size: 14px;
  }

  img {
    height: 30px;
    width: 30px;
  }
}

@media (max-width: 480px) {
  .footer-container {
    padding: 0 10px;
    flex-direction: column;
    gap: 20px;
  }

  .contact-info h2,
  .social-links h2 {
    font-size: 18px;
  }

  .contact-info p {
    font-size: 18px;
  }

  img {
    height: 25px;
    width: 25px;
  }

  .icon {
    font-size: 20px;
  }
}
</style>
