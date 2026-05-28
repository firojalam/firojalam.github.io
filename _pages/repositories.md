---
layout: page
permalink: /repositories/
title: repositories
description: Selected Hugging Face and GitHub repositories
nav: true
nav_order: 4
---

{% assign repository_users = site.data.repositories.github_users | join: "|" | replace: "firojalam|", "" | replace: "|firojalam", "" | replace: "firojalam", "" | split: "|" %}
{% assign huggingface_groups = site.data.repositories.huggingface_groups %}
{% assign huggingface_repos = site.data.repositories.huggingface_repos %}
{% assign github_repos = site.data.repositories.github_repos %}
{% assign total_repositories = huggingface_repos.size | plus: github_repos.size %}

<div class="repo-layout">
  <div class="repo-overview">
    <a class="repo-section-link" href="#hf-repositories">
      <span class="repo-section-link__kicker">Section 1</span>
      <strong class="repo-section-link__title">HF</strong>
      <span class="repo-section-link__meta">{{ huggingface_repos.size }} datasets and models with live download totals</span>
    </a>
    <a class="repo-section-link" href="#git-repositories">
      <span class="repo-section-link__kicker">Section 2</span>
      <strong class="repo-section-link__title">Git</strong>
      <span class="repo-section-link__meta">{{ github_repos.size }} selected repositories plus GitHub profile context</span>
    </a>
  </div>

  <div class="repo-toolbar">
    <label class="repo-toolbar__label" for="repo-search">Find a repository</label>
    <input
      class="repo-toolbar__input"
      id="repo-search"
      type="search"
      placeholder="Search by name, topic, owner, or description"
      autocomplete="off"
      data-repo-search
    >
    <p class="repo-toolbar__summary" data-repo-search-summary>Showing all {{ total_repositories }} repositories.</p>
  </div>
</div>

<section id="hf-repositories" class="repo-section">
  <div class="repo-section__header">
    <div>
      <h2>HF</h2>
      <p class="repo-section__copy">
        Datasets and models, grouped by research line so visitors can scan related resources without jumping across the full page.
      </p>
    </div>
    <div class="repo-chip-list">
      {% for group in huggingface_groups %}
        <a class="repo-chip" href="#{{ group.name | slugify }}">{{ group.name }}</a>
      {% endfor %}
    </div>
  </div>

  {% for group in huggingface_groups %}
    {% assign group_repos = huggingface_repos | where: "group", group.name %}
    {% if group_repos.size > 0 %}
      <section id="{{ group.name | slugify }}" class="repo-topic js-repo-group">
        <div class="repo-topic__header">
          <h3>{{ group.name }}</h3>
          <p>{{ group.description }}</p>
        </div>
        <div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-stretch">
          {% for repo in group_repos %}
            {% include repository/repo_hf.liquid repository=repo %}
          {% endfor %}
        </div>
      </section>
    {% endif %}
  {% endfor %}
</section>

<section id="git-repositories" class="repo-section">
  <div class="repo-section__header">
    <div>
      <h2>Git</h2>
      <p class="repo-section__copy">
        GitHub profiles, recent activity, and selected repositories. GitHub does not expose a general repository download total, so this section keeps stars, forks, and language instead.
      </p>
    </div>
  </div>

  {% if repository_users %}
    <div class="repo-subsection">
      <h3>Profiles</h3>
      <div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-stretch">
        {% for user in repository_users %}
          {% include repository/repo_user.liquid username=user %}
        {% endfor %}
      </div>
    </div>

    {% if site.repo_trophies.enabled %}
      <div class="repo-subsection">
        <h3>Recent activity</h3>
        {% for user in repository_users %}
          {% if repository_users.size > 1 %}
            <h4 class="repo-subsection__label">@{{ user }}</h4>
          {% endif %}
          <div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-stretch">
            {% include repository/repo_trophies.liquid username=user %}
          </div>
        {% endfor %}
      </div>
    {% endif %}
  {% endif %}

  <div class="repo-subsection js-repo-group">
    <h3>Selected repositories</h3>
    {% if github_repos %}
      <div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-stretch">
        {% for repo in github_repos %}
          {% include repository/repo.liquid repository=repo %}
        {% endfor %}
      </div>
    {% endif %}
  </div>
</section>

<script>
  (() => {
    const githubApiRoot = "https://api.github.com";
    const huggingFaceApiRoot = "https://huggingface.co/api";
    const numberFormatter = new Intl.NumberFormat("en-US");
    const dateFormatter = new Intl.DateTimeFormat("en", { month: "short", day: "numeric", year: "numeric" });
    const shortDateFormatter = new Intl.DateTimeFormat("en", { month: "short", year: "numeric" });
    const cache = new Map();
    const searchableCards = Array.from(document.querySelectorAll(".js-repo-card"));
    const filterGroups = Array.from(document.querySelectorAll(".js-repo-group"));
    const searchInput = document.querySelector("[data-repo-search]");
    const searchSummary = document.querySelector("[data-repo-search-summary]");

    async function getJson(url, headers = {}) {
      const cacheKey = `${url}::${JSON.stringify(headers)}`;

      if (!cache.has(cacheKey)) {
        cache.set(
          cacheKey,
          fetch(url, {
            headers,
          }).then((response) => {
            if (!response.ok) {
              throw new Error(`API ${response.status}`);
            }
            return response.json();
          })
        );
      }

      return cache.get(cacheKey);
    }

    function formatNumber(value) {
      return typeof value === "number" ? numberFormatter.format(value) : "--";
    }

    function formatDate(value) {
      if (!value) {
        return "--";
      }

      return dateFormatter.format(new Date(value));
    }

    function formatMonthYear(value) {
      if (!value) {
        return "--";
      }

      return shortDateFormatter.format(new Date(value));
    }

    function clampText(value, fallback) {
      const text = typeof value === "string" ? value.trim() : "";
      return text || fallback;
    }

    function cleanSummary(value, fallback) {
      const text = typeof value === "string" ? value.replace(/\s+/g, " ").replace(/^#+\s*/, "").trim() : "";
      return text || fallback;
    }

    function setField(card, field, value) {
      const element = card.querySelector(`[data-field="${field}"]`);
      if (element) {
        element.textContent = value;
      }
    }

    function getHuggingFaceUrl(repoId, kind) {
      if (!repoId) {
        return "https://huggingface.co";
      }

      return kind === "Dataset" ? `https://huggingface.co/datasets/${repoId}` : `https://huggingface.co/${repoId}`;
    }

    function normalizeSearchValue(value) {
      if (typeof value !== "string") {
        return "";
      }

      return value
        .toLowerCase()
        .replace(/[^a-z0-9]+/g, " ")
        .trim();
    }

    function setSearchText(card, values) {
      card.dataset.search = values
        .map((value) => normalizeSearchValue(value))
        .filter(Boolean)
        .join(" ");
    }

    function applyRepositoryFilter() {
      const query = normalizeSearchValue(searchInput?.value || "");
      let visibleCount = 0;

      searchableCards.forEach((card) => {
        const wrapper = card.closest(".repo");
        if (!wrapper) {
          return;
        }

        const matches = !query || (card.dataset.search || "").includes(query);
        wrapper.hidden = !matches;

        if (matches) {
          visibleCount += 1;
        }
      });

      filterGroups.forEach((group) => {
        group.hidden = !group.querySelector(".repo:not([hidden])");
      });

      if (searchSummary) {
        searchSummary.textContent = query
          ? `Showing ${visibleCount} matching repositories.`
          : `Showing all ${searchableCards.length} repositories.`;
      }
    }

    async function populateUserCard(card) {
      const username = card.dataset.githubUser;
      if (!username) {
        return;
      }

      try {
        const user = await getJson(`${githubApiRoot}/users/${username}`, {
          Accept: "application/vnd.github+json",
        });
        const avatar = card.querySelector(".repo-card__avatar");

        if (avatar && user.avatar_url) {
          avatar.src = user.avatar_url;
          avatar.alt = user.login || username;
        }

        setField(card, "title", clampText(user.name, user.login || username));
        setField(card, "subtitle", `@${user.login || username}`);
        setField(card, "followers", formatNumber(user.followers));
        setField(card, "following", formatNumber(user.following));
        setField(card, "repos", formatNumber(user.public_repos));

        const description = card.querySelector(".repo-card__description");
        if (description) {
          description.textContent = clampText(
            user.bio,
            user.company ? `Works at ${user.company}` : "Open GitHub profile."
          );
        }
      } catch (error) {
        const description = card.querySelector(".repo-card__description");
        if (description) {
          description.textContent = "GitHub profile details are unavailable right now.";
        }
        card.classList.add("repo-card--error");
      }
    }

    async function populateRepoCard(card) {
      const fullName = card.dataset.githubRepo;
      if (!fullName) {
        return;
      }

      try {
        const repo = await getJson(`${githubApiRoot}/repos/${fullName}`, {
          Accept: "application/vnd.github+json",
        });
        const avatar = card.querySelector(".repo-card__avatar");
        const showOwner = card.dataset.showOwner === "true";

        if (avatar && repo.owner && repo.owner.avatar_url) {
          avatar.src = repo.owner.avatar_url;
          avatar.alt = repo.owner.login || repo.full_name;
        }

        setField(card, "title", clampText(repo.name, fullName));
        setField(card, "subtitle", showOwner ? repo.full_name : `@${repo.owner.login}`);
        setField(card, "stars", formatNumber(repo.stargazers_count));
        setField(card, "forks", formatNumber(repo.forks_count));
        setField(card, "language", clampText(repo.language, "n/a"));

        const description = card.querySelector(".repo-card__description");
        if (description) {
          description.textContent = clampText(repo.description, "Open repository on GitHub.");
        }

        setSearchText(card, [repo.full_name, repo.name, repo.description, repo.language, repo.owner?.login]);
        applyRepositoryFilter();
      } catch (error) {
        const description = card.querySelector(".repo-card__description");
        if (description) {
          description.textContent = "Repository details are unavailable right now.";
        }
        card.classList.add("repo-card--error");
      }
    }

    async function getHuggingFaceRepo(repoId) {
      const hfExpandParams = "expand=downloadsAllTime&expand=downloads&expand=likes&expand=lastModified";
      const candidates = [
        { kind: "Dataset", url: `${huggingFaceApiRoot}/datasets/${repoId}?${hfExpandParams}` },
        { kind: "Model", url: `${huggingFaceApiRoot}/models/${repoId}?${hfExpandParams}` },
      ];
      let lastError = null;

      for (const candidate of candidates) {
        try {
          const payload = await getJson(candidate.url);
          return { payload, kind: candidate.kind };
        } catch (error) {
          lastError = error;
        }
      }

      throw lastError || new Error("Hugging Face API error");
    }

    async function populateHuggingFaceCard(card) {
      const repoId = card.dataset.hfRepo;
      if (!repoId) {
        return;
      }

      try {
        const { payload, kind } = await getHuggingFaceRepo(repoId);
        const title = clampText(payload.id?.split("/").pop(), repoId.split("/").pop());

        card.href = getHuggingFaceUrl(clampText(payload.id, repoId), kind);
        setField(card, "title", title);
        setField(card, "subtitle", clampText(payload.id, repoId));
        setField(card, "kind", kind);
        setField(card, "downloads", formatNumber(payload.downloadsAllTime ?? payload.downloads));
        setField(card, "likes", formatNumber(payload.likes));
        setField(card, "updated", formatMonthYear(payload.lastModified));

        const description = card.querySelector(".repo-card__description");
        if (description) {
          description.textContent = cleanSummary(
            payload.description,
            kind === "Model" ? "Open model card on Hugging Face." : "Open dataset card on Hugging Face."
          );
        }

        setSearchText(card, [payload.id, title, payload.description, kind, payload.author, payload.pipeline_tag, payload.tags?.join(" ")]);
        applyRepositoryFilter();
      } catch (error) {
        const description = card.querySelector(".repo-card__description");
        if (description) {
          description.textContent = "Hugging Face details are unavailable right now.";
        }
        setField(card, "kind", "Unavailable");
        card.classList.add("repo-card--error");
      }
    }

    function renderRecentRepo(repo) {
      const item = document.createElement("li");
      item.className = "repo-card__list-item";

      const link = document.createElement("a");
      link.className = "repo-card__list-link";
      link.href = repo.html_url;
      link.target = "_blank";
      link.rel = "external nofollow noopener";

      const name = document.createElement("span");
      name.className = "repo-card__list-title";
      name.textContent = repo.name;

      const meta = document.createElement("span");
      meta.className = "repo-card__list-meta";
      meta.textContent = `${formatNumber(repo.stargazers_count)} stars • updated ${formatDate(repo.updated_at)}`;

      link.append(name, meta);
      item.append(link);
      return item;
    }

    async function populateRecentCard(card) {
      const username = card.dataset.githubRecentUser;
      if (!username) {
        return;
      }

      try {
        const repos = await getJson(`${githubApiRoot}/users/${username}/repos?sort=updated&per_page=6&type=owner`, {
          Accept: "application/vnd.github+json",
        });
        const list = card.querySelector(".repo-card__list");
        const candidates = Array.isArray(repos) ? repos.filter((repo) => !repo.fork).slice(0, 3) : [];

        if (!list) {
          return;
        }

        list.innerHTML = "";

        if (!candidates.length) {
          const emptyItem = document.createElement("li");
          emptyItem.className = "repo-card__list-item repo-card__list-item--muted";
          emptyItem.textContent = "No recent repositories available.";
          list.append(emptyItem);
          return;
        }

        candidates.forEach((repo) => list.append(renderRecentRepo(repo)));
      } catch (error) {
        const list = card.querySelector(".repo-card__list");
        if (list) {
          list.innerHTML = '<li class="repo-card__list-item repo-card__list-item--muted">Recent repositories are unavailable right now.</li>';
        }
        card.classList.add("repo-card--error");
      }
    }

    searchInput?.addEventListener("input", applyRepositoryFilter);
    applyRepositoryFilter();

    document.querySelectorAll(".js-hf-repo-card").forEach((card) => {
      populateHuggingFaceCard(card);
    });

    document.querySelectorAll(".js-github-user-card").forEach((card) => {
      populateUserCard(card);
    });

    document.querySelectorAll(".js-github-repo-card").forEach((card) => {
      populateRepoCard(card);
    });

    document.querySelectorAll(".js-github-recent-card").forEach((card) => {
      populateRecentCard(card);
    });
  })();
</script>
