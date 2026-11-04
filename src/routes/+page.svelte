<script>
  /**
   * Copy for the maintenance page, kept as typed data so it can be tweaked
   * in one place.
   * @typedef {{ title: string, heading: string, blurb: string, tagline: string, footer: string }} Copy
   */

  /** @type {Copy} */
  const copy = {
    title: 'Under Maintenance | Playjeux Studios',
    heading: "We're Under Maintenance",
    blurb:
      "Our website is temporarily unavailable while we work on getting things back online. Thank you for your patience. We'll be back soon.",
    tagline: 'We Are Multiplayer',
    footer: '© 2026 Playjeux Studios Inc.'
  };

  /**
   * True once the logo image fails to load, which swaps in the text wordmark.
   * @type {boolean}
   */
  let logoFailed = $state(false);
</script>

<svelte:head>
  <title>{copy.title}</title>
  <meta name="robots" content="noindex, nofollow" />
  <meta name="description" content="Our website is temporarily unavailable." />
</svelte:head>

<main>
  <!-- Drop your official Playjeux Studios logo here as src/static/playjeux-logo.png -->
  {#if !logoFailed}
    <img
      class="logo"
      src="/playjeux-logo.png"
      alt="Playjeux Studios"
      onerror={() => (logoFailed = true)}
    />
  {:else}
    <div class="brand">PLAYJEUX <span>STUDIOS</span></div>
  {/if}

  <h1>{copy.heading}</h1>
  <p>{copy.blurb}</p>

  <div class="rule"></div>
  <div class="tag">{copy.tagline}</div>

  <footer>{copy.footer}</footer>
</main>

<style>
  :global(body) {
    box-sizing: border-box;
    margin: 0;
    min-height: 100vh;
    display: grid;
    place-items: center;
    background: radial-gradient(
      ellipse at 50% 25%,
      #24204a 0%,
      #0e1024 50%,
      #070a15 100%
    );
    font-family: Inter, Arial, Helvetica, sans-serif;
    color: #f7f8ff;
    text-align: center;
    padding: 32px;
  }

  :global(:root) {
    color-scheme: dark;
  }

  main {
    max-width: 720px;
    width: 100%;
  }

  .logo {
    display: block;
    max-width: min(380px, 85%);
    max-height: 145px;
    width: auto;
    height: auto;
    object-fit: contain;
    margin: 0 auto 64px;
  }

  .brand {
    font-weight: 800;
    font-size: clamp(30px, 5vw, 52px);
    letter-spacing: 0.04em;
    margin-bottom: 60px;
  }
  .brand span {
    color: #b9aaff;
  }

  h1 {
    font-size: clamp(38px, 6vw, 68px);
    letter-spacing: -0.045em;
    line-height: 1.08;
    margin: 0 0 25px;
    font-weight: 750;
  }

  p {
    font-size: clamp(17px, 2.1vw, 21px);
    line-height: 1.7;
    color: #c5c7db;
    margin: 0 auto;
    max-width: 600px;
  }

  .rule {
    height: 3px;
    width: 66px;
    background: #8568fa;
    border-radius: 3px;
    margin: 40px auto;
  }

  .tag {
    font-size: 13px;
    letter-spacing: 0.22em;
    text-transform: uppercase;
    color: #b9aaff;
    font-weight: 700;
  }

  footer {
    margin-top: 85px;
    color: #8387a7;
    font-size: 13px;
  }
</style>