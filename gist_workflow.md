<table>
  <tr>
    <td><img src="https://gist.githubusercontent.com/iamAntimPal/a109f4a19e3df1501c6331be75a06cea/raw/activity.svg" alt="Activity" /></td>
    <td><img src="https://gist.githubusercontent.com/iamAntimPal/a109f4a19e3df1501c6331be75a06cea/raw/metrics.plugin.code.svg" alt="Code" /></td>
  </tr>
  <tr>
    <td><img src="https://gist.githubusercontent.com/iamAntimPal/a109f4a19e3df1501c6331be75a06cea/raw/metrics.plugin.followup.svg" alt="Follow-Up" /></td>
    <td><img src="https://gist.githubusercontent.com/iamAntimPal/a109f4a19e3df1501c6331be75a06cea/raw/metrics.plugin.issues.svg" alt="Issues" /></td>
  </tr>
  <tr>
    <td><img src="https://gist.githubusercontent.com/iamAntimPal/a109f4a19e3df1501c6331be75a06cea/raw/metrics.plugin.repositories.svg" alt="Repositories" /></td>
    <td><img src="https://gist.githubusercontent.com/iamAntimPal/a109f4a19e3df1501c6331be75a06cea/raw/metrics.plugin.reactions.svg" alt="Reactions" /></td>
  </tr>
</table>



### This is the main SVG

```yml
# .github/workflows/metrics.yml
name: Github_Metrics
on:
  # Daily at 16:00 UTC
  schedule:
    - cron: "0 16 * * *"
  # Manual trigger from the Actions tab
  workflow_dispatch:

permissions:
  contents: write     # to push updates / create PRs
  actions: read       # to run actions

jobs:
  metrics:
    runs-on: ubuntu-latest

    steps:
      # 1️⃣ Checkout your profile repo
      - name: 🦑 Checkout repo
        uses: actions/checkout@v4
        with:
          fetch-depth: 0
          persist-credentials: false

      # 2️⃣ Install jq (required by lowlighter/metrics)
      - name: 🦑 Install jq
        run: |
          sudo apt-get update
          sudo apt-get install -y jq

      # 3️⃣ Followup
      - name: 🦑 Followup
        if: ${{ success() || failure() }}
        uses: lowlighter/metrics@latest
        with:
          filename: followup.svg
          token: ${{ secrets.METRICS_TOKEN }}
          base: ""
          output_action: gist
          committer_gist: ${{ secrets.WORKFLOW_GIST }}
          plugin_followup: yes
          plugin_followup_sections: repositories, user
          plugin_followup_indepth: yes
          plugin_followup_archived: no


      # 4️⃣ Achievements
      - name: 🦑 Achievements
        if: ${{ success() || failure() }}
        uses: lowlighter/metrics@latest
        with:
          filename: achievements.svg
          token: ${{ secrets.METRICS_TOKEN }}
          base: ""
          output_action: gist
          committer_gist: ${{ secrets.WORKFLOW_GIST }}
          plugin_achievements: yes
          plugin_achievements_display: compact
          plugin_fortune: yes



      # 5️⃣ Sponsors
      - name: 🦑 Sponsors
        if: ${{ success() || failure() }}
        uses: lowlighter/metrics@latest
        with:
          filename: sponsors.svg
          token: ${{ secrets.METRICS_TOKEN }}
          base: ""
          output_action: gist
          committer_gist: ${{ secrets.WORKFLOW_GIST }}
          plugin_sponsors: yes
          plugin_sponsors_past: yes
          plugin_sponsorships: yes
          plugin_sponsorships_sections: amount
          config_timezone: Asia/Kolkata

      # 6️⃣ Languages
      - name: 🦑 Languages
        if: ${{ success() || failure() }}
        uses: lowlighter/metrics@latest
        with:
          filename: languages.svg
          token: ${{ secrets.METRICS_TOKEN }}
          base: ""
          # base_indepth: yes
          # config_order: base.header, isocalendar, languages, notable, discussions, topics
          output_action: gist
          committer_gist: ${{ secrets.WORKFLOW_GIST }}
          # plugin_isocalendar: yes
          plugin_languages: yes
          plugin_languages_ignored: html, css, tex, less, dockerfile, makefile, qmake, lex, cmake, shell, gnuplot, vue, scala, c, c++, ejs
          plugin_languages_details: lines, bytes-size, percentage
          plugin_languages_sections: most-used, recently-used
          plugin_languages_indepth: yes
          plugin_languages_limit: 6
          plugin_topics: yes
          plugin_topics_limit: 10
          plugin_topics_mode: icons
          #plugin_notable: yes
          #plugin_discussions: yes
          config_timezone: Asia/Kolkata

      # 7️⃣ discussions
      - name: 🦑 Discussions
        if: ${{ success() || failure() }}
        uses: lowlighter/metrics@latest
        with:
          filename: discussions.svg
          token: ${{ secrets.METRICS_TOKEN }}
          base: ""
          output_action: gist
          committer_gist: ${{ secrets.WORKFLOW_GIST }}
          plugin_discussions: yes
          plugin_discussions_categories_limit: 12
          plugin_discussions_categories: yes
          config_timezone: Asia/Kolkata

      # 8️⃣ **NEW** Reaction
      - name: 🦑 Reactions
        if: ${{ success() || failure() }}
        uses: lowlighter/metrics@latest
        with:
          filename: Reactions.svg
          token: ${{ secrets.METRICS_TOKEN }}
          base: ""
          output_action: gist
          committer_gist: ${{ secrets.WORKFLOW_GIST }}
          plugin_reactions: yes
          plugin_reactions_limit: 100
          plugin_reactions_details: percentage

      # 8️⃣ **NEW** Lines of Code Changed
      - name: 🐙 Lines of Code Changed
        if: ${{ success() || failure() }}
        uses: lowlighter/metrics@latest
        with:
          filename: lines.svg
          token: ${{ secrets.METRICS_TOKEN }}
          base: ""
          output_action: gist
          committer_gist: ${{ secrets.WORKFLOW_GIST }}
          plugin_lines: yes
          plugin_lines_sections: base, repositories,
          # skip any repos you don’t want included:
          plugin_lines_skipped: testing, The-Futurists_Manthan_Frontend
          # limit the breakdown to top 10 repos:
          plugin_lines_repositories_limit: 50
          config_timezone: Asia/Kolkata
          
      # 9️⃣ LeetCode 
      - name: 📊 Generate LeetCode Metrics
        if: ${{ success() || failure() }}
        uses: lowlighter/metrics@latest
        with:
          filename: leetcode.svg
          token: ${{ secrets.METRICS_TOKEN }}
          base: ""
          output_action: gist
          committer_gist: ${{ secrets.WORKFLOW_GIST }}
          plugin_leetcode: yes
          plugin_leetcode_user: antim_pal
          plugin_leetcode_sections: solved, skills, recent
          config_timezone: Asia/Kolkata

      # Contribution
      - name: 🐙Contribution
        if: ${{ success() || failure() }}
        uses: lowlighter/metrics@latest
        with:
          filename: contribution.svg
          token: ${{ secrets.METRICS_TOKEN }}
          base: ""
          output_action: gist
          committer_gist: ${{ secrets.WORKFLOW_GIST }}
          template: repository
          repo: TechInterviewMaster
          plugin_contributors: yes
          plugin_contributors_contributions: yes
          config_timezone: Asia/Kolkata


      # Code
      - name: 🐙Code
        if: ${{ success() || failure() }}
        uses: lowlighter/metrics@latest
        with:
          filename: code.svg
          token: ${{ secrets.METRICS_TOKEN }}
          base: ""
          output_action: gist
          committer_gist: ${{ secrets.WORKFLOW_GIST }}
          plugin_code: yes
          plugin_code_lines: 12
          plugin_code_visibility: public
          plugin_code_languages: python, html, 
          

      # Topics
      - name: 📊 Generate
        if: ${{ success() || failure() }}
        uses: lowlighter/metrics@latest
        with:
          filename: topics.svg
          token: ${{ secrets.METRICS_TOKEN }}
          base: ""
          output_action: gist
          committer_gist: ${{ secrets.WORKFLOW_GIST }}
          plugin_topics: yes
          plugin_topics_limit: 10
          plugin_topics_sort: star, activity
          plugin_topics_mode: icons
          config_timezone: Asia/Kolkata

      # 9️⃣ Commit & push all SVG updates to a PR
      - name: 🦑 Commit & Create PR
        uses: peter-evans/create-pull-request@v6
        with:
          token: ${{ secrets.METRICS_TOKEN }}
          branch: metrics-update
          title: "chore: update all metrics SVGs"
          body: "Auto-generated metrics via lowlighter/metrics"
```



## SVG IN ROOT BAGSE AND ALL

```yml
# Visit https://github.com/lowlighter/metrics#-documentation for full reference
name: Metrics
on:
   # Schedule updates (daily)
   schedule: [{ cron: "0 0 * * *" }]
   # Lines below let you run workflow manually and on each commit
   workflow_dispatch:
   push: { branches: ["master", "main"] }
jobs:
   github-metrics:
      runs-on: ubuntu-latest
      permissions:
         contents: write
      steps:
         - uses: lowlighter/metrics@latest
           with:
              # Your GitHub token
              # The following scopes are required:
              #  - public_access (default scope)
              # The following additional scopes may be required:
              #  - read:org      (for organization related metrics)
              #  - read:user     (for user related data)
              #  - read:packages (for some packages related data)
              #  - repo          (optional, if you want to include private repositories)
              token: ${{ secrets.METRICS_TOKEN }}

              # Options
              user: iamAntimPal
              template: classic
              base: header, activity, community, repositories, metadata
              config_timezone: Germany/Berlin
              plugin_isocalendar: yes
              plugin_isocalendar_duration: full-year
              plugin_languages: yes
              plugin_languages_analysis_timeout: 15
              plugin_languages_analysis_timeout_repositories: 7.5
              plugin_languages_categories: programming
              plugin_languages_ignored: jupyter notebook, shell, tex, html, css, dockerfile, makefile, cmake, vhdl, php, hcl, hs, java, powershell, assembly
              plugin_languages_colors: github
              plugin_languages_limit: 8
              plugin_languages_recent_categories: programming
              plugin_languages_recent_days: 14
              plugin_languages_recent_load: 300
              plugin_languages_sections: most-used
              plugin_languages_threshold: 0%
              plugin_topics: yes
              plugin_topics_limit: 0
              plugin_topics_mode: icons
              plugin_notable: yes
              plugin_discussions: yes
              plugin_discussions_categories_limit: 8

```
