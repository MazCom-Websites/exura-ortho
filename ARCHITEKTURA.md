# Architektura projektu ExuraOrtho

> Dokumentacja techniczna dla repozytorium treści (`exura-ortho`).
> Kod aplikacji: `/Users/karol/WebstormProjects/exuraortho`

---

## Dwa repozytoria

| Repozytorium | Opis |
|---|---|
| `exura-ortho` | **Tylko treść** — pliki `.json` w `outstatic/content/` |
| `exuraortho` | **Aplikacja Next.js 16** — czyta treść z `exura-ortho` |

---

## Stack techniczny (aplikacja)

- **Framework:** Next.js 16 App Router (`next@^16.1.6`)
- **React:** 19
- **Język:** TypeScript (strict)
- **Stylizacja:** Tailwind CSS 4 (`@tailwindcss/postcss`, `@tailwindcss/typography`)
- **Animacje:** Framer Motion 12, Embla Carousel 8
- **Ikony:** Lucide React
- **Walidacja:** Zod 4
- **Formularze:** React Hook Form 7 + Server Actions
- **Email:** Resend
- **Markdown:** react-markdown, next-mdx-remote, remark-gfm, rehype-sanitize
- **Testy:** Vitest 4 + Testing Library (unit), Playwright (E2E)
- **Inne:** next-themes, clsx, tailwind-merge, @dnd-kit, @octokit/rest, axios

---

## Format treści

Cała treść jest przechowywana jako **pliki JSON** (nie Markdown). Każdy plik to jeden dokument `ContentDocument`.

### Konwencja nazewnictwa plików

```
outstatic/content/
├── {slug}.json                        # Strona nadrzędna (parent page)
├── {parentType}--{childSlug}.json     # Strona podrzędna (child page)
└── theme.json                         # Kolory motywu (nie URL)
```

Przykłady:
- `home.json` — strona główna (`path: "/"`)
- `uslugi.json` — listing usług (`path: "/uslugi"`)
- `uslugi--fizjoterapia.json` — podstrona usługi (`parentPath: "/uslugi"`, `slug: "fizjoterapia"`)
- `pracownicy--anna-kowalska.json` — profil pracownika
- `blog--bol-kregoslupa-przyczyny-leczenie.json` — post blogowy

---

## Struktura `outstatic/content/`

### Strony nadrzędne (17 + theme)

| Plik | path | Opis |
|---|---|---|
| `home.json` | `/` | Strona główna |
| `o-nas.json` | `/o-nas` | O firmie, historia, wartości |
| `uslugi.json` | `/uslugi` | Listing usług (`contextType: "services"`) |
| `pracownicy.json` | `/pracownicy` | Listing pracowników (`contextType: "teamMembers"`) |
| `blog.json` | `/blog` | Listing postów (`contextType: "blogPosts"`) |
| `kontakt.json` | `/kontakt` | Formularz kontaktowy + dane |
| `rezerwacja.json` | `/rezerwacja` | Rezerwacja wizyty (`renderMode: "with-form"`) |
| `faq.json` | `/faq` | Najczęściej zadawane pytania |
| `cennik.json` | `/cennik` | Cennik usług |
| `rehabilitacja.json` | `/rehabilitacja` | Informacje o rehabilitacji |
| `sprzet-medyczny.json` | `/sprzet-medyczny` | Sprzęt medyczny |
| `galeria.json` | `/galeria` | Galeria gabinetu i zespołu |
| `dla-firm.json` | `/dla-firm` | Oferta B2B dla firm |
| `dofinansowanie.json` | `/dofinansowanie` | Dofinansowanie NFZ/PFRON |
| `opinie.json` | `/opinie` | Opinie pacjentów |
| `polityka-prywatnosci.json` | `/polityka-prywatnosci` | Polityka prywatności |
| `regulamin.json` | `/regulamin` | Regulamin |
| `theme.json` | — | Kolory motywu (nie URL) |

### Usługi (12 podstron)

| Plik | slug |
|---|---|
| `uslugi--fizjoterapia.json` | fizjoterapia |
| `uslugi--rehabilitacja-domowa.json` | rehabilitacja-domowa |
| `uslugi--terapia-manualna.json` | terapia-manualna |
| `uslugi--masaz-leczniczy.json` | masaz-leczniczy |
| `uslugi--sprzet-medyczny.json` | sprzet-medyczny |
| `uslugi--doradztwo-sprzet.json` | doradztwo-sprzet |
| `uslugi--diagnostyka-funkcjonalna.json` | diagnostyka-funkcjonalna |
| `uslugi--fizykoterapia.json` | fizykoterapia |
| `uslugi--kinezyterapia.json` | kinezyterapia |
| `uslugi--taping.json` | taping |
| `uslugi--terapia-powieziowa.json` | terapia-powieziowa |
| `uslugi--zaopatrzenie-ortopedyczne.json` | zaopatrzenie-ortopedyczne |

### Pracownicy (6 podstron)

| Plik | slug |
|---|---|
| `pracownicy--piotr-lewandowski.json` | piotr-lewandowski |
| `pracownicy--anna-kowalska.json` | anna-kowalska |
| `pracownicy--marek-wisniewski.json` | marek-wisniewski |
| `pracownicy--katarzyna-nowak.json` | katarzyna-nowak |
| `pracownicy--tomasz-zielinski.json` | tomasz-zielinski |
| `pracownicy--magdalena-kaczmarek.json` | magdalena-kaczmarek |

### Posty blogowe (10 podstron)

| Plik | slug |
|---|---|
| `blog--fizjoterapia-sportowa-kontuzje.json` | fizjoterapia-sportowa-kontuzje |
| `blog--jak-przygotowac-sie-do-rehabilitacji.json` | jak-przygotowac-sie-do-rehabilitacji |
| `blog--sprzet-rehabilitacyjny-poradnik.json` | sprzet-rehabilitacyjny-poradnik |
| `blog--bol-kregoslupa-przyczyny-leczenie.json` | bol-kregoslupa-przyczyny-leczenie |
| `blog--cwiczenia-na-bol-plecow.json` | cwiczenia-na-bol-plecow |
| `blog--dofinansowanie-nfz-poradnik.json` | dofinansowanie-nfz-poradnik |
| `blog--kinesiotaping-zastosowania.json` | kinesiotaping-zastosowania |
| `blog--rehabilitacja-domowa-kiedy-warto.json` | rehabilitacja-domowa-kiedy-warto |
| `blog--rehabilitacja-po-operacji-kolana.json` | rehabilitacja-po-operacji-kolana |
| `blog--terapia-manualna-co-to-jest.json` | terapia-manualna-co-to-jest |

---

## Routing aplikacji

Jeden catch-all route obsługuje wszystkie strony:

```
app/(site)/
└── [[...slug]]/page.tsx    # Rozpoznaje parent pages (wg path) i child pages (wg parentPath + slug)
```

- `dynamicParams: false` — tylko wygenerowane ścieżki, reszta → 404
- `dynamic: 'force-static'` — pełna statyczna generacja

### Rozwiązywanie dokumentu

1. Szuka strony nadrzędnej z polem `path` pasującym do URL
2. Jeśli URL ma 2+ segmenty — szuka strony podrzędnej z `parentPath` + `slug`

---

## Tryby renderowania (renderMode)

| Tryb | Opis | Użycie |
|---|---|---|
| `slices` | Renderuje tablicę slice'ów z JSON | Domyślny dla większości stron |
| `blog-post` | Widok postu blogowego (metadane, tagi, reading time, powiązane posty) | Posty blogowe |
| `richtext` | Renderuje pole `content` jako Markdown/prose | Polityka prywatności, regulamin |
| `with-form` | Slice'y + `AppointmentForm` (formularz rezerwacji) | `/rezerwacja` |

Tryb jest ustalany wg: `doc.renderMode` → `doc.pageType` → `parent.childRenderMode` → `'slices'`

---

## System kontekstu (context)

Slice'y mogą potrzebować danych z innych dokumentów. System kontekstu dostarcza je automatycznie.

### Strony nadrzędne z `contextType`
Pole `contextType` na stronie nadrzędnej powoduje załadowanie jej podstron do kontekstu:
- `uslugi.json` → `contextType: "services"` → `context.services` = wszystkie usługi
- `blog.json` → `contextType: "blogPosts"` → `context.blogPosts` = wszystkie posty
- `pracownicy.json` → `contextType: "teamMembers"` → `context.teamMembers` = wszyscy pracownicy

### Strony podrzędne
Automatycznie otrzymują:
- `context.service` / `context.member` — sam dokument (do użycia przez slice'y hero/detail)
- `context.siblings` — rodzeństwo (do nawigacji)
- `context.relatedServices` — powiązane usługi (z pola `relatedServices`)
- `context.relatedPosts` — powiązane posty (z pola `relatedPosts`)

### Auto-detekcja
Jeśli slice JSON zawiera np. `services_grid`, a kontekst nie ma jeszcze `services` — automatycznie je załaduje. Analogicznie dla `blog_grid`, `team_profile_grid` i innych slice'ów zespołu.

---

## Domyślne slice'y

Strony podrzędne bez własnych slice'ów (`slices` puste lub `[]`) otrzymują domyślny zestaw:

### Usługi (SERVICE_DEFAULT_SLICES — 8 slice'ów)
1. `service_hero`
2. `service_description`
3. `service_indications`
4. `service_contraindications`
5. `service_process`
6. `service_pricing`
7. `service_related`
8. `cta_section`

### Pracownicy (TEAM_DEFAULT_SLICES — 8 slice'ów)
1. `employee_hero`
2. `employee_stats`
3. `employee_expertise`
4. `employee_approach`
5. `employee_certifications`
6. `employee_schedule`
7. `team_related`
8. `cta_section`

---

## Unified ContentDocument type

Wszystkie dokumenty (strony, usługi, pracownicy, posty) mają jeden typ `ContentDocument`:

```typescript
interface ContentDocument {
  // Tożsamość
  title: string;
  status: 'published' | 'draft';
  publishedAt: string;

  // Strona nadrzędna (ma path)
  path?: string;
  contextKey?: string;
  contextType?: string;
  childRenderMode?: string;

  // Strona podrzędna (ma slug + parentPath)
  slug?: string;
  parentPath?: string;

  // Renderowanie
  renderMode?: 'slices' | 'blog-post' | 'richtext' | 'with-form';
  slices?: SliceBase[] | string;
  content?: string;

  // SEO
  metadata?: CmsMetadata;

  // Pozostałe pola (usługi, zespół, blog) — free-form
  [key: string]: unknown;
}
```

### Pola usług (parentPath: "/uslugi")

```json
{
  "name": "Fizjoterapia",
  "slug": "fizjoterapia",
  "shortDescription": "...",
  "icon": "Heart",
  "mainImage": "https://...",
  "category": "Fizjoterapia",
  "duration": 50,
  "price": 150,
  "priceFrom": false,
  "popular": true,
  "indications": [{ "indication": "Bóle kręgosłupa", "icon": "Check" }],
  "contraindications": [{ "contraindication": "..." }],
  "processSteps": [{ "title": "...", "description": "..." }],
  "pricingVariants": [{ "name": "...", "duration": "50 min", "price": "150 zł", "description": "..." }],
  "relatedServices": ["terapia-manualna", "masaz-leczniczy"]
}
```

### Pola pracowników (parentPath: "/pracownicy")

```json
{
  "name": "Anna Kowalska",
  "slug": "anna-kowalska",
  "position": "Specjalista Fizjoterapii Sportowej",
  "shortBio": "Krótkie bio do kart",
  "bio": "Pełne bio",
  "photo": "https://...",
  "email": "anna@exuraortho.pl",
  "phone": "",
  "experienceYears": 10,
  "specializations": ["Rehabilitacja sportowa", "Kinezytaping"],
  "education": [
    { "type": "education", "year": "2013", "title": "Magistr Fizjoterapii", "institution": "AWF Gdańsk" },
    { "type": "certification", "year": "2015", "title": "Kinesiology Taping", "institution": "..." }
  ],
  "certifications": ["Cert 1", "Cert 2"],
  "relatedServices": ["fizjoterapia", "taping"]
}
```

### Pola postów blogowych (parentPath: "/blog")

```json
{
  "slug": "bol-kregoslupa-przyczyny-leczenie",
  "title": "Ból kręgosłupa — przyczyny i leczenie",
  "excerpt": "Krótkie streszczenie...",
  "coverImage": "https://...",
  "category": "Rehabilitacja",
  "tags": [{ "tag": "kręgosłup" }, { "tag": "ból" }],
  "readingTime": 8,
  "featured": true,
  "authorName": "Piotr Lewandowski",
  "authorBio": "...",
  "authorImage": "https://...",
  "relatedService": "fizjoterapia",
  "relatedPosts": ["cwiczenia-na-bol-plecow"],
  "content": "# Treść w Markdown...",
  "renderMode": "blog-post"
}
```

### SEO (CmsMetadata)

```json
{
  "metadata": {
    "metaTitle": "...",
    "metaDescription": "...",
    "metaImage": "https://...",
    "ogImage": "https://...",
    "robots": "index,follow",
    "keywords": "fizjoterapia, rehabilitacja",
    "canonicalUrl": "/uslugi/fizjoterapia"
  }
}
```

---

## System slice'ow (138 komponentow)

Slice'y to komponenty sekcji strony. Dane przechowywane jako JSON w polu `slices`.

### Zapis w JSON
```json
[
  {
    "slice_type": "gradient_mesh_hero",
    "primary": { "title": "...", "subtitle": "..." },
    "items": [{ "tag": "Specjalizacja" }]
  }
]
```

### Konwencja kluczy
- `slice_type` w JSON = klucz w snake_case
- Komponent w `src/slices/NazwaSlice/index.tsx`
- Rejestr: `src/slices/index.ts`

---

## Dostepne slice'y — pelna lista (138)

### Hero (10)
| slice_type | Opis |
|---|---|
| `animated_text_hero` | Hero z animowanym rotujacym tekstem |
| `gradient_mesh_hero` | Hero z gradientem mesh |
| `split_hero` | Hero dwukolumnowy (tekst + obraz) |
| `split_reveal_hero` | Hero z animacja reveal |
| `text_title_hero` | Prosty hero z tytulem i opisem |
| `hero_centered` | Hero wycentrowany |
| `countdown_hero` | Hero z odliczaniem |
| `glitch_text_hero` | Hero z efektem glitch |
| `particle_field_hero` | Hero z animacja czastek |
| `scroll_reveal_hero` | Hero odslanianany przy scrollu |
| `typewriter_hero` | Hero z efektem maszyny do pisania |

### Social proof / Opinie (9)
| slice_type | Opis |
|---|---|
| `trust_badges` | Pasek statystyk z ikonami |
| `featured_testimonial` | Wyrosniona opinia pacjenta |
| `testimonials_carousel` | Karuzela opinii |
| `testimonial_masonry` | Masonry layout opinii |
| `testimonial_wall` | Sciana opinii |
| `testimonial_spotlight` | Spotlight na opinie |
| `video_testimonials` | Opinie wideo |
| `social_proof_counter` | Licznik social proof |
| `review_platforms` | Agregacja opinii z platform |

### Funkcje / Sekcje tresciowe (17)
| slice_type | Opis |
|---|---|
| `feature_alternating` | Naprzemienne sekcje lewo/prawo |
| `feature_matrix` | Siatka funkcji |
| `floating_features` | Plywajace/sticky funkcje |
| `sticky_scroll_features` | Funkcje przyklejane przy scrollu |
| `icon_card_grid` | Siatka kart z ikonami |
| `numbered_features` | Numerowana lista funkcji |
| `bento_grid` | Layout bento box |
| `animated_icon_features` | Animowane ikony z trescia |
| `comparison_columns` | Kolumny porownawcze |
| `expanding_cards` | Karty rozwijane |
| `reveal_on_scroll_cards` | Karty odslaniane przy scrollu |
| `tilt_cards` | Karty z efektem przechylenia |
| `glassmorphism_cards` | Karty glassmorphism |
| `value_cards_slider` | Slider kart wartosci |
| `value_proposition` | Sekcja propozycji wartosci |
| `why_choose_us` | Dlaczego my |
| `amenities_grid` | Siatka udogodnien |

### Uslugi (14)
| slice_type | Opis |
|---|---|
| `services_grid` | Siatka uslug (context.services) |
| `service_hero` | Hero strony uslugi (context.service) |
| `service_description` | Opis uslugi |
| `service_indications` | Wskazania do terapii |
| `service_contraindications` | Przeciwwskazania |
| `service_pricing` | Cennik uslugi |
| `service_process` | Kroki procesu uslugi |
| `service_related` | Powiazane uslugi |
| `service_comparison` | Porownanie uslug |
| `service_comparison_table` | Tabela porownawcza |
| `service_showcase_tabs` | Zakladki showcase uslug |
| `service_card_stack` | Stos kart uslug |
| `service_hover_reveal` | Uslugi z hover reveal |
| `service_flip_cards` | Karty flip uslug |
| `service_timeline` | Timeline uslug |
| `equipment_showcase` | Showcase sprzetu |

### Pracownicy (13)
| slice_type | Opis |
|---|---|
| `team_profile_grid` | Siatka kart pracownikow z linkami |
| `team_grid` | Podstawowa siatka zespolu |
| `team_detail_cards` | Szczegolowe karty zespolu |
| `team_carousel_3d` | Karuzela 3D zespolu |
| `team_hex_grid` | Siatka hexagonalna |
| `team_filter_grid` | Filtrowalna siatka zespolu |
| `team_timeline_intro` | Wprowadzenie zespolu timeline |
| `team_stats_bio` | Zespol ze statystykami |
| `team_related` | Pozostali specjalisci (nawigacja) |
| `employee_hero` | Hero profilu pracownika (context.member) |
| `employee_stats` | Statystyki pracownika |
| `employee_expertise` | Obszary specjalizacji |
| `employee_approach` | "Moje podejscie do terapii" |
| `employee_certifications` | Wyksztalcenie i certyfikaty |
| `employee_schedule` | Harmonogram + CTA |

### Blog (8)
| slice_type | Opis |
|---|---|
| `blog_grid` | Siatka postow (context.blogPosts) |
| `blog_list` | Lista postow |
| `blog_carousel_cards` | Karuzela kart blogu |
| `featured_article_hero` | Hero wyroznionego artykulu |
| `top_articles_sidebar` | Sidebar z top artykulami |
| `category_showcase` | Showcase kategorii blogu |
| `content_marquee` | Przewijany marquee tresci |
| `reading_progress_article` | Artykul z paskiem postepu |

### Kontakt / Lokalizacja (8)
| slice_type | Opis |
|---|---|
| `contact_form_with_details` | Formularz + dane kontaktowe |
| `contact_info_cards` | Karty informacji kontaktowych |
| `contact_cards` | Stylizowane karty kontaktu |
| `map_location` | Mapa + adres |
| `opening_hours_card` | Godziny otwarcia |
| `location_with_directions` | Mapa z dojazdem |
| `multi_step_contact_form` | Wielokrokowy formularz |
| `appointment_scheduler` | Planer wizyt |

### Call-to-Action (8)
| slice_type | Opis |
|---|---|
| `cta_section` | Standardowy baner CTA |
| `glow_cta` | CTA ze swieceniem |
| `sticky_cta` | Przyklejone CTA na dole |
| `urgency_cta` | CTA z pilnoscia |
| `split_screen_cta` | CTA split screen |
| `floating_booking_cta` | Plywajacy CTA rezerwacji |
| `exit_intent_cta` | CTA na wyjscie |
| `appointment_banner` | Baner rezerwacji wizyty |

### FAQ / Wiedza (5)
| slice_type | Opis |
|---|---|
| `faq_accordion` | Akordeon FAQ |
| `searchable_faq` | FAQ z wyszukiwarka |
| `treatment_faq` | FAQ o zabiegach |
| `interactive_knowledge_base` | Interaktywna baza wiedzy |
| `guided_troubleshooter` | Prowadzony troubleshooter |

### Galeria / Multimedia (8)
| slice_type | Opis |
|---|---|
| `image_gallery` | Galeria zdjec |
| `masonry_gallery` | Galeria masonry |
| `parallax_gallery` | Galeria z parallaxem |
| `lightbox_grid` | Siatka z lightboxem |
| `before_after_gallery` | Galeria przed/po |
| `image_comparison_grid` | Siatka porownawcza obrazow |
| `video_showcase_reel` | Showcase wideo |
| `horizontal_scroll_showcase` | Poziomy scroll showcase |

### Wideo (3)
| slice_type | Opis |
|---|---|
| `video_embed` | Osadzony odtwarzacz wideo |
| `video_hero` | Hero z wideo w tle |
| `video_testimonials` | Karuzela opinii wideo |

### Statystyki (6)
| slice_type | Opis |
|---|---|
| `stats_grid` | Siatka statystyk |
| `animated_counter_bar` | Animowany pasek licznikow |
| `progress_bars_section` | Paski postepu |
| `stats_reveal_cards` | Karty statystyk z reveal |
| `infographic_stats` | Infografika statystyk |
| `milestone_counter` | Licznik kamieni milowych |

### Medyczne (3)
| slice_type | Opis |
|---|---|
| `medical_specialties` | Siatka specjalizacji medycznych |
| `patient_journey` | Wizualizacja drogi pacjenta |
| `pricing_table` | Tabela cennikowa |

### Nawigacja / UI (5)
| slice_type | Opis |
|---|---|
| `header_navigation` | Nawigacja naglowka |
| `navigation` | Glowna nawigacja |
| `breadcrumb_progress` | Okruszki z postepem |
| `page_section_nav` | Nawigacja sekcji strony |
| `scroll_progress_bar` | Pasek postepu czytania |
| `sticky_tab_navigation` | Sticky zakladki nawigacji |

### Pozostale (12)
| slice_type | Opis |
|---|---|
| `accordion_with_media` | Akordeon z mediami |
| `alert_banner` | Baner alertu/powiadomienia |
| `certifications_row` | Rzad certyfikatow |
| `logo_marquee` | Karuzela logo |
| `newsletter` | Formularz zapisu na newsletter |
| `parallax_banner` | Baner z efektem parallax |
| `partners_grid` | Siatka partnerow |
| `process_steps` | Timeline procesu |
| `promo_text_cta` | Tekstowa promocja CTA |
| `next_item_cta` | CTA do nastepnego elementu |
| `quote_block` | Blok cytatu |
| `resource_downloads` | Zasoby do pobrania |
| `text_content` | Blok tresci tekstowej |
| `timeline` | Os czasu |
| `animated_logo_cloud` | Animowana chmura logo |
| `awards_showcase` | Showcase nagrod |

---

## Kolory motywu

Zdefiniowane w `outstatic/content/theme.json` i renderowane przez `src/app/globals.css` z dyrektywą `@theme inline` (Tailwind 4).

- `primary-*` — kolor teal (#008f8c / #0ab8b5)
- `secondary-*` — zielony (#22c55e)
- `accent-*` — pomaranczowy (#f97316)
- `neutral-*` (nigdy `gray-*`)
- Font: DM Sans

---

## Jak zaktualizowac strone

1. Edytuj odpowiedni plik `.json` w `outstatic/content/`
2. Zmien treść w polach lub w tablicy `slices`
3. `git add`, `git commit`, `git push`
4. Aplikacja przebuduje strony automatycznie

---

## Jak dodac nowa strone nadrzedna

1. Utwórz plik `outstatic/content/nowa-strona.json`
2. Ustaw `"path": "/nowa-strona"`
3. Wypełnij `"slices": [...]` z wybranymi komponentami

## Jak dodac nowa usluge

1. Utwórz `outstatic/content/uslugi--nazwa-uslugi.json`
2. Ustaw `"parentPath": "/uslugi"`, `"slug": "nazwa-uslugi"`
3. Wypełnij pola: `name`, `shortDescription`, `icon`, `category`, `duration`, `price`, `indications`, itd.
4. Opcjonalnie dodaj `"slices": [...]` — bez nich usluga dostanie domyslny zestaw slice'ow

## Jak dodac nowego pracownika

1. Utwórz `outstatic/content/pracownicy--imie-nazwisko.json`
2. Ustaw `"parentPath": "/pracownicy"`, `"slug": "imie-nazwisko"`
3. Wypelnij pola: `name`, `position`, `shortBio`, `bio`, `photo`, `email`, `experienceYears`, `specializations`, `education`
4. Opcjonalnie dodaj `"slices": [...]` — bez nich profil dostanie domyslny zestaw slice'ow

## Jak dodac nowy post blogowy

1. Utwórz `outstatic/content/blog--slug-posta.json`
2. Ustaw `"parentPath": "/blog"`, `"slug": "slug-posta"`, `"renderMode": "blog-post"`
3. Wypelnij pola: `title`, `excerpt`, `coverImage`, `category`, `tags`, `readingTime`, `authorName`, `content`
4. Pole `content` zawiera tresc w Markdown
