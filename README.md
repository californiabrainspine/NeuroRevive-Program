<?php
/**
 * Plugin Name: NeuroRevive Page Override (MU)
 * Description: Overrides the content and SEO meta for the NeuroRevive Program page with stylish, SEO-optimized markup, schema, and a dofollow link.
 * Author: Amir SEO Ops
 * Version: 1.0.0
 */

// ==== CONFIG ====
const NR_PAGE_SLUG = 'neurorevive-program'; // slug of the target page
const NR_SITE_NAME = 'California Brain & Spine Center';
const NR_SITE_URL  = 'https://californiabrainspine.com';
const NR_PAGE_URL  = 'https://californiabrainspine.com/services/neurorevive-program/';
const NR_PHONE     = '+1-818-649-5300';
const NR_ADDRESS   = '4768 Park Granada #107, Calabasas, CA 91302';
const NR_LOGO      = 'https://californiabrainspine.com/wp-content/uploads/logo.png'; // optional logo URL
const NR_IMG       = 'https://californiabrainspine.com/wp-content/uploads/neurorevive-hero.jpg'; // optional hero image
const NR_DESC      = 'NeuroRevive™ is a precision neuro-rehabilitation program in Calabasas that integrates Neurosensory Integration, vestibular/oculomotor retraining, and autonomic regulation to help patients recover from concussion, dizziness, dysautonomia (POTS), and chronic brain fog.';

// Helpers
function nr_is_target_page(): bool {
    if (is_admin()) return false;
    if (!is_page()) return false;
    $page = get_queried_object();
    return isset($page->post_name) && $page->post_name === NR_PAGE_SLUG;
}

// ===== Replace Page Content =====
add_filter('the_content', function ($content) {
    if (!nr_is_target_page()) return $content;

    ob_start(); ?>
    <article id="neurorevive" class="nr-wrap">
      <section class="nr-hero">
        <div class="nr-hero-inner">
          <h1 class="nr-title">NeuroRevive™ Program — Precision Neuro-Rehabilitation in Calabasas</h1>
          <p class="nr-subtitle">From “normal scans” to feeling normal again: targeted recovery for concussion, dizziness, dysautonomia (POTS), and brain fog.</p>
          <div class="nr-cta">
            <a class="nr-btn" href="<?php echo esc_url(NR_SITE_URL); ?>">Book an Evaluation</a>
            <a class="nr-btn ghost" href="#nr-how-it-works">How It Works</a>
          </div>
        </div>
        <div class="nr-decor" aria-hidden="true"></div>
      </section>

      <section class="nr-section nr-grid">
        <div class="nr-card">
          <h2>What Is NeuroRevive™?</h2>
          <p><strong>NeuroRevive™</strong> is a structured, measurement-driven program led by Dr. Alireza Chizari at <a href="<?php echo esc_url(NR_SITE_URL); ?>"><?php echo esc_html(NR_SITE_NAME); ?></a>. We integrate <em>Neurosensory Integration (NSI)</em>, vestibular/oculomotor retraining, autonomic conditioning, and sensorimotor progressions to restore real-world function.</p>
          <ul class="nr-list">
            <li>Concussion / Post-Concussion Syndrome</li>
            <li>Vestibular Disorders &amp; Chronic Dizziness</li>
            <li>Dysautonomia (including POTS)</li>
            <li>Migraine with autonomic/vestibular features</li>
          </ul>
        </div>
        <div class="nr-card">
          <h2 id="nr-how-it-works">How It Works</h2>
          <ol class="nr-steps">
            <li><strong>Deep Functional Assessment:</strong> oculomotor metrics, balance &amp; gait, autonomic screens.</li>
            <li><strong>Personalized Protocol:</strong> graded vestibular/oculomotor drills, sensory integration, autonomic pacing.</li>
            <li><strong>Adaptive Progression:</strong> frequent re-tests; dosage, rest, and complexity adjust to your threshold.</li>
            <li><strong>Real-Life Transfer:</strong> milestones tied to study/work/sport and everyday stability.</li>
          </ol>
        </div>
        <div class="nr-card">
          <h2>Why Patients Choose Us</h2>
          <ul class="nr-bullets">
            <li>Measurement-driven: see objective change, not just “feel” it.</li>
            <li>Time-intensive sessions with clear pacing &amp; recovery windows.</li>
            <li>Calabasas location for consistent follow-through (West Valley/Conejo corridor).</li>
            <li>Compassionate team culture: curiosity, clarity, and coaching.</li>
          </ul>
        </div>
      </section>

      <section class="nr-section nr-highlight">
        <h2>Who Benefits Most</h2>
        <p>If you’re weeks or months post-injury and still feel dizzy, foggy, off-balance—or standing triggers orthostatic symptoms—NeuroRevive™ gives your nervous system a structured path to adapt and stabilize.</p>
        <div class="nr-cta">
          <a class="nr-btn" href="<?php echo esc_url(NR_SITE_URL); ?>">Schedule Now</a>
        </div>
      </section>

      <section class="nr-section nr-faq">
        <h2>FAQ</h2>
        <details><summary>How long until I notice improvement?</summary><p>Timelines vary, but many patients notice early shifts in stability, stamina, or visual comfort within the first 2–4 weeks of consistent work.</p></details>
        <details><summary>Do you coordinate with my neurologist/ENT/cardiology?</summary><p>Yes. We routinely coordinate testing and communication across specialties, especially for vestibular and autonomic presentations.</p></details>
        <details><summary>Is this a replacement for imaging or medication?</summary><p>No. NeuroRevive™ complements appropriate medical care. We address functional performance so your real life feels normal again.</p></details>
      </section>

      <section class="nr-section nr-contact">
        <h2>Contact</h2>
        <p><strong><?php echo esc_html(NR_SITE_NAME); ?></strong><br><?php echo esc_html(NR_ADDRESS); ?><br><?php echo esc_html(NR_PHONE); ?></p>
        <p>Learn more at <a href="<?php echo esc_url(NR_SITE_URL); ?>"><?php echo esc_html(NR_SITE_URL); ?></a></p>
      </section>
    </article>
    <?php
    return ob_get_clean();
}, 999);

// ===== Inject Meta/OG/Twitter/Canonical & JSON-LD =====
add_action('wp_head', function () {
    if (!nr_is_target_page()) return;

    // Basic meta
    echo "\n<!-- NeuroRevive Override Meta -->\n";
    printf('<meta name="description" content="%s" />'."\n", esc_attr(NR_DESC));
    echo '<meta name="robots" content="index, follow" />'."\n";
    printf('<link rel="canonical" href="%s" />'."\n", esc_url(NR_PAGE_URL));

    // Open Graph
    printf('<meta property="og:title" content="%s" />'."\n", esc_attr('NeuroRevive™ Program — Precision Neuro-Rehabilitation in Calabasas'));
    printf('<meta property="og:description" content="%s" />'."\n", esc_attr(NR_DESC));
    printf('<meta property="og:url" content="%s" />'."\n", esc_url(NR_PAGE_URL));
    printf('<meta property="og:site_name" content="%s" />'."\n", esc_attr(NR_SITE_NAME));
    if (NR_IMG) printf('<meta property="og:image" content="%s" />'."\n", esc_url(NR_IMG));

    // Twitter
    echo '<meta name="twitter:card" content="summary_large_image" />'."\n";
    printf('<meta name="twitter:title" content="%s" />'."\n", esc_attr('NeuroRevive™ Program — Precision Neuro-Rehabilitation in Calabasas'));
    printf('<meta name="twitter:description" content="%s" />'."\n", esc_attr(NR_DESC));
    if (NR_IMG) printf('<meta name="twitter:image" content="%s" />'."\n", esc_url(NR_IMG));

    // JSON-LD: Organization + Service + FAQPage (minimal, valid)
    $json = [
        '@context' => 'https://schema.org',
        '@graph' => [
            [
                '@type' => 'Organization',
                'name'  => NR_SITE_NAME,
                'url'   => NR_SITE_URL,
                'logo'  => NR_LOGO,
                'telephone' => NR_PHONE,
                'address' => [
                    '@type' => 'PostalAddress',
                    'streetAddress' => '4768 Park Granada #107',
                    'addressLocality' => 'Calabasas',
                    'addressRegion' => 'CA',
                    'postalCode' => '91302',
                    'addressCountry' => 'US'
                ]
            ],
            [
                '@type' => 'Service',
                'name'  => 'NeuroRevive™ Program',
                'url'   => NR_PAGE_URL,
                'provider' => [
                    '@type' => 'MedicalClinic',
                    'name'  => NR_SITE_NAME,
                    'url'   => NR_SITE_URL
                ],
                'areaServed' => [
                    '@type' => 'AdministrativeArea',
                    'name'  => 'Calabasas, West San Fernando Valley, Conejo Valley'
                ],
                'description' => NR_DESC,
                'serviceType' => 'Neurorehabilitation'
            ],
            [
                '@type' => 'FAQPage',
                'mainEntity' => [
                    [
                        '@type' => 'Question',
                        'name' => 'How long until I notice improvement?',
                        'acceptedAnswer' => [
                            '@type' => 'Answer',
                            'text' => 'Timelines vary, but many patients notice early shifts in stability, stamina, or visual comfort within the first 2–4 weeks of consistent work.'
                        ]
                    ],
                    [
                        '@type' => 'Question',
                        'name' => 'Do you coordinate with my neurologist/ENT/cardiology?',
                        'acceptedAnswer' => [
                            '@type' => 'Answer',
                            'text' => 'Yes. We coordinate testing and communication across specialties, especially for vestibular and autonomic presentations.'
                        ]
                    ],
                    [
                        '@type' => 'Question',
                        'name' => 'Is this a replacement for imaging or medication?',
                        'acceptedAnswer' => [
                            '@type' => 'Answer',
                            'text' => 'No. NeuroRevive™ complements appropriate medical care. We address functional performance so daily life feels normal again.'
                        ]
                    ]
                ]
            ]
        ]
    ];
    echo '<script type="application/ld+json">'.wp_json_encode($json).'</script>'."\n";
}, 20);

// ===== Styles (clean, attractive, fast) =====
add_action('wp_enqueue_scripts', function () {
    if (!nr_is_target_page()) return;

    // Create a dummy handle to attach inline CSS
    wp_register_style('nr-inline', false);
    wp_enqueue_style('nr-inline');

    $css = <<<CSS
.nr-wrap{--bg:#0f1226;--ink:#f7f8ff;--muted:#b7bde2;--acc:#7c9cff;--acc2:#5ef3c2;--card:#171a37;--glow:0 10px 40px rgba(124,156,255,.25);color:var(--ink);font-family:ui-sans-serif,system-ui,-apple-system,Segoe UI,Roboto,Inter,Ubuntu,Helvetica,Arial,sans-serif;}
.nr-hero{background:radial-gradient(1200px 600px at 90% 10%,rgba(124,156,255,.25),transparent 60%),linear-gradient(160deg,#0f1226 0%,#131739 50%,#0f1226 100%);padding:72px 24px 56px;position:relative;overflow:hidden}
.nr-hero-inner{max-width:1100px;margin:0 auto;text-align:center}
.nr-title{font-size:clamp(28px,5vw,44px);line-height:1.1;margin:0 0 12px;font-weight:800;letter-spacing:-.02em}
.nr-subtitle{font-size:clamp(16px,2.2vw,20px);color:var(--muted);max-width:900px;margin:0 auto 28px}
.nr-cta{display:flex;gap:12px;justify-content:center;flex-wrap:wrap}
.nr-btn{display:inline-block;padding:14px 20px;border-radius:14px;background:linear-gradient(135deg,var(--acc),var(--acc2));color:#0b0e1f;font-weight:700;text-decoration:none;box-shadow:var(--glow);transition:transform .15s ease}
.nr-btn:hover{transform:translateY(-1px)}
.nr-btn.ghost{background:transparent;color:var(--ink);border:1px solid rgba(255,255,255,.18)}
.nr-decor:before{content:"";position:absolute;inset:auto -20% -40% -20%;height:200px;background:radial-gradient(60% 120% at 50% 0%,rgba(94,243,194,.18),transparent 70%)}
.nr-section{max-width:1100px;margin:40px auto;padding:0 24px}
.nr-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(260px,1fr));gap:18px}
.nr-card{background:var(--card);border:1px solid rgba(255,255,255,.08);border-radius:18px;padding:22px 20px;box-shadow:0 6px 30px rgba(0,0,0,.25)}
.nr-card h2{margin:.2rem 0 8px;font-size:22px}
.nr-list,.nr-bullets{margin:10px 0 0 16px}
.nr-steps{counter-reset:step;margin:10px 0 0 0;padding:0}
.nr-steps li{list-style:none;margin:10px 0;padding-left:34px;position:relative}
.nr-steps li:before{counter-increment:step;content:counter(step);position:absolute;left:0;top:0;width:26px;height:26px;border-radius:8px;background:linear-gradient(135deg,var(--acc),var(--acc2));color:#0b0e1f;font-weight:800;display:grid;place-items:center}
.nr-highlight{background:linear-gradient(180deg,rgba(124,156,255,.08),transparent);border-radius:22px;padding:24px 20px;border:1px solid rgba(255,255,255,.06)}
.nr-faq details{background:rgba(255,255,255,.03);border:1px solid rgba(255,255,255,.08);border-radius:14px;padding:14px 16px;margin:10px 0}
.nr-faq summary{cursor:pointer;font-weight:700}
.nr-contact{display:grid;place-items:center;text-align:center;background:linear-gradient(140deg,rgba(124,156,255,.12),rgba(94,243,194,.08));border-radius:22px}
@media (prefers-color-scheme: light){
  .nr-wrap{--bg:#f6f7ff;--ink:#0e1233;--muted:#46507a;--card:#ffffff}
  .nr-hero{background:linear-gradient(160deg,#e9ecff 0%,#eef0ff 50%,#e9ecff 100%)}
  .nr-btn{color:#07122a}
}
CSS;
    wp_add_inline_style('nr-inline', $css);
}, 20);

// (Optional) Ensure page stays public/indexable (safety)
add_filter('pre_option_blog_public', function ($value) {
    return $value; // respect site setting; remove this if you force indexability globally
});
