# HTML
Application format of tags and general design cases

## basic website structure ~[application based]~

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="Styling/" rel="stylesheet">
    <!-- best practice option #1-->
   <!-- <script src="" defer ></script> -->
    <title>Synetrix</title>
    
</head>

<body>
    <header>
        <div class = "brand">
        <img src="images/logo.png" alt="Logo"> Brand    </div>
        
        <nav class="Horisontal-nav">
            <a href="">Home</a>
            <a href="">About</a>
            <a href="">Services</a>
            <a href="">Contact</a>
        </nav>
        <!-- Handburger menu -->
         <button class="Burger-menu">
            <span></span>
            <span></span>
            <span></span>
         </button>
    </header>

    <main>
        <Section class="front-page">
            <h2>Design, Build, Create and Innovate</h2>
            <img src="" alt="">
        </Section>

        <section class="About-me">
            <h2></h2>
            <img src="" alt="">
            <h3></h3>
            <p></p>
            
        </section>

        <section class="projects">
            <div class="spinn-projection"></div>
        </section>

        <section class="Contact">

        </section>
    </main>
    <footer>
        <footer class="footer">
    <div class="footer-top">
        <div class="footer-brand">
            <h2>YourBrand</h2>
            <p>Short tagline or mission statement.</p>
        </div>

        <div class="footer-columns">
            <div class="footer-column">
                <h3>Product</h3>
                <a href="#">Features</a>
                <a href="#">Pricing</a>
                <a href="#">Integrations</a>
                <a href="#">Demo</a>
            </div>

            <div class="footer-column">
                <h3>Company</h3>
                <a href="#">About Us</a>
                <a href="#">Careers</a>
                <a href="#">Blog</a>
                <a href="#">Press</a>
            </div>

            <div class="footer-column">
                <h3>Resources</h3>
                <a href="#">Help Center</a>
                <a href="#">Documentation</a>
                <a href="#">Community</a>
                <a href="#">Status</a>
            </div>

            <div class="footer-column">
                <h3>Contact</h3>
                <a href="mailto:info@yourbrand.com">info@yourbrand.com</a>
                <a href="tel:+1234567890">+1 234 567 890</a>
                <a href="#">Location</a>
            </div>
        </div>
    </div>

    <div class="footer-bottom">
        <div class="social-links">
            <a href="#" aria-label="LinkedIn">LinkedIn</a>
            <a href="#" aria-label="Instagram">Instagram</a>
            <a href="#" aria-label="Twitter">Twitter</a>
        </div>

        <div class="legal-links">
            <a href="#">Privacy Policy</a>
            <a href="#">Terms of Service</a>
            <a href="#">Cookie Policy</a>
        </div>

        <p class="copyright">
            © <span id="year"></span> YourBrand. All rights reserved.
        </p>
    </div>
</footer>

<script>
    document.getElementById("year").textContent = new Date().getFullYear();
</script>

    </footer>
<!-- best practice option #2-->
<script src=""></script>
</body>
</html>
```
# References

