# Architektura projektu ExuraOrtho

> Dokumentacja techniczna dla repozytorium treści (`exura-ortho`).
> Kod aplikacji: `/Users/karol/WebstormProjects/exuraortho`

---

## Dwa repozytoria

| Repozytorium | Opis |
|---|---|
| `exura-ortho` | **Tylko treść** — pliki `.md` w `outstatic/content/` |
| `exuraortho` | **Aplikacja Next.js 15** — czyta treść z `exura-ortho` |

---

## Stack techniczny (aplikacja)

- **Framework:** Next.js 15 App Router
- **Język:** TypeScript (strict)
- **Stylizacja:** Tailwind CSS 4 (utility-first, mobile-first)
- **CMS:** Outstatic (czyta `.md` z `outstatic/content/`)
- **Walidacja:** Zod
- **Formularze:** React Hook Form + Server Actions

---

## Struktura `outstatic/content/`

```
outstatic/content/
├── pages/          # Strony singleton (każda = jedna URL)
│   ├── home.md           → /
│   ├── about.md          → /o-nas
│   ├── services-page.md  → /uslugi
│   ├── pracownicy.md     → /pracownicy
│   ├── contact.md        → /kontakt
│   ├── faq.md            → /faq
│   ├── blog-page.md      → /blog
│   ├── rezerwacja.md     → /rezerwacja
│   ├── rehabilitacja.md  → /rehabilitacja
│   ├── sprzet-medyczny.md → /sprzet-medyczny
│   ├── polityka-prywatnosci.md → /polityka-prywatnosci
│   ├── regulamin.md      → /regulamin
│   └── theme.md          → kolory motywu (nie URL)
├── services/       # Kolekcja usług → /uslugi/[slug]
│   ├── fizjoterapia.md
│   ├── rehabilitacja-domowa.md
│   ├── terapia-manualna.md
│   ├── masaz-leczniczy.md
│   ├── sprzet-medyczny.md
│   └── doradztwo-sprzet.md
├── team/           # Kolekcja pracowników → /pracownicy/[slug]
│   ├── piotr-lewandowski.md
│   ├── anna-kowalska.md
│   ├── marek-wisniewski.md
│   ├── katarzyna-nowak.md
│   ├── tomasz-zielinski.md
│   └── magdalena-kaczmarek.md
└── blog-posts/     # Kolekcja postów → /blog/[slug]
    ├── fizjoterapia-sportowa-kontuzje.md
    ├── jak-przygotowac-sie-do-rehabilitacji.md
    └── sprzet-rehabilitacyjny-poradnik.md
```

---

## Routing aplikacji

```
app/(site)/
├── [[...slug]]/page.tsx    # Catch-all: czyta z pages/ wg pola `path`
├── uslugi/[uid]/page.tsx   # Czyta z services/ wg slug
├── pracownicy/[uid]/page.tsx # Czyta z team/ wg slug
└── blog/[uid]/page.tsx     # Czyta z blog-posts/ wg slug
```

---

## Frontmatter stron (`pages/`)

```yaml
title: 'Tytuł strony'
status: 'published'          # lub 'draft'
publishedAt: '2024-01-01T00:00:00.000Z'
path: '/url-strony'          # URL strony
pageType: 'slices'           # 'slices' | 'richtext' | 'with-form'
contextType: ''              # 'services' | 'blog' | ''
slices: '[...]'              # JSON z tablicą slice'ów
metaTitle: '...'
metaDescription: '...'
```

---

## Frontmatter pracowników (`team/`)

```yaml
title: 'Imię Nazwisko'
slug: 'imie-nazwisko'         # kebab-case
publishedAt: '2024-01-01T00:00:00.000Z'
status: 'published'
name: 'Imię Nazwisko'
position: 'Stanowisko'
shortBio: 'Krótkie bio (do kart listujących)'
bio: 'Pełne bio'
photo: '/images/team/imie-nazwisko.jpg'
email: 'imie@exuraortho.pl'
phone: '+48 xxx xxx xxx'
experienceYears: '10'
specializations: '["Spec 1","Spec 2"]'    # JSON array
education: '[{"type":"education|certification|course","year":"2020","title":"...","institution":"..."}]'
certifications: '["Cert 1","Cert 2"]'     # JSON array
relatedServices: '["fizjoterapia","terapia-manualna"]'
slices: '[...]'              # JSON z pełną treścią strony profilu
metaTitle: '...'
metaDescription: '...'
```

---

## System slice'ów

Slice'y to komponenty sekcji strony. Dane przechowywane jako JSON w polu `slices` frontmatter.

### Zapis w CMS
```json
[
  {
    "slice_type": "employee_hero",
    "primary": { "name": "...", "position": "..." },
    "items": [{ "tag": "Specjalizacja" }]
  }
]
```

### Konwencja kluczy
- `slice_type` w JSON = nazwa w snake_case
- Komponent w `src/slices/NazwaSlice/index.tsx`
- Rejestr: `src/slices/index.ts`

---

## Dostępne slice'y (kluczowe)

### Strony ogólne
| slice_type | Opis |
|---|---|
| `animated_text_hero` | Hero z animowanym rotującym tekstem |
| `split_hero` | Hero dwukolumnowy (tekst + obraz) |
| `text_title_hero` | Prosty hero z tytułem i opisem |
| `trust_badges` | Pasek statystyk z ikonami |
| `cta_section` | Baner call-to-action |
| `featured_testimonial` | Wyróżniona opinia pacjenta |
| `testimonials_carousel` | Karuzela opinii |
| `faq_accordion` | Akordeon FAQ |
| `blog_grid` | Siatka postów blogowych |
| `newsletter` | Formularz zapisu na newsletter |
| `parallax_banner` | Baner z efektem parallax |
| `timeline` | Oś czasu (historia firmy) |

### Usługi
| slice_type | Opis |
|---|---|
| `services_grid` | Siatka usług z context.services |
| `service_hero` | Hero strony usługi (z context.service) |
| `service_description` | Opis usługi |
| `service_indications` | Wskazania do terapii |
| `service_pricing` | Cennik usługi |
| `service_related` | Powiązane usługi |

### Pracownicy
| slice_type | Opis |
|---|---|
| `team_profile_grid` | Siatka kart pracowników z linkami do profili |
| `employee_hero` | Hero profilu pracownika (foto, bio, kontakt) |
| `employee_stats` | Statystyki pracownika |
| `employee_expertise` | Obszary specjalizacji |
| `employee_approach` | "Moje podejście do terapii" |
| `employee_certifications` | Wykształcenie i certyfikaty |
| `employee_schedule` | Harmonogram + CTA |
| `team_related` | Pozostali specjaliści (poziomy pasek) |

### Kontakt
| slice_type | Opis |
|---|---|
| `contact_form_with_details` | Formularz kontaktowy + dane |
| `map_location` | Mapa + adres |
| `opening_hours_card` | Godziny otwarcia |

---

## Kolory motywu

Zdefiniowane w `outstatic/content/pages/theme.md`. Zmiana koloru = edycja tam.

- `primary-*` — kolor teal (#008f8c domyślnie)
- `secondary-*`, `accent-*`
- `neutral-*` (nigdy `gray-*`)

---

## Jak zaktualizować stronę

1. Edytuj odpowiedni plik `.md` w `outstatic/content/`
2. Zmień treść w frontmatter (pola lub JSON w `slices`)
3. `git add`, `git commit`, `git push`
4. Aplikacja przebuduje strony automatycznie

---

## Jak dodać nową stronę

1. Utwórz plik `outstatic/content/pages/nowa-strona.md`
2. Ustaw `path: '/nowa-strona'`
3. Wypełnij `slices` JSON z wybranymi komponentami

## Jak dodać nowego pracownika

1. Utwórz `outstatic/content/team/imie-nazwisko.md`
2. Wypełnij wszystkie pola frontmatter
3. W `slices` JSON umieść: `employee_hero`, `employee_stats`, `employee_expertise`, `employee_approach`, `employee_certifications`, `employee_schedule`, `team_related`, `cta_section`
4. Zaktualizuj `pracownicy.md` — dodaj wpis w `slice.items` slice'a `team_profile_grid`
5. Zaktualizuj sekcję `team_related.items` w plikach pozostałych pracowników
