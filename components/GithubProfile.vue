<template>
  <div class="github-card" style="margin-top: 0.5rem;">
    <div class="card-split">

      <!-- Left: Profile -->
      <div class="profile-panel">
        <img src="https://avatars.githubusercontent.com/u/50106623?v=4" class="avatar" />
        <span class="name">Robin Quintero</span>
        <span class="handle">rohaquinlop</span>
        <span class="position">Software Engineer II @ GenLogs</span>
<span class="speaking">Python Cali 2024 · PyCon Colombia 2025</span>
      </div>

      <!-- Right: Projects -->
      <div class="projects-panel">

        <!-- Pinned repos -->
        <div class="row" style="align-items: center; gap: 6px; margin-bottom: 8px;">
          <span style="width: 8px; height: 8px; border-radius: 50%; background: #e6edf3; display: inline-block;"></span>
          <span style="color: #e6edf3; font-weight: 600; font-size: 0.82rem;">Destacados</span>
        </div>

        <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-bottom: 10px;">

          <!-- complexipy -->
          <div class="repo-card">
            <div class="row" style="align-items: center; gap: 5px; margin-bottom: 3px;">
              <span class="repo-name">complexipy</span>
            </div>
            <div class="repo-desc" style="margin-bottom: 5px;">Blazingly fast cognitive complexity analysis for Python, written in Rust</div>
            <div class="row muted" style="align-items: center; font-size: 0.68rem;">
              <span class="row" style="align-items: center; gap: 4px;">
                <span class="lang-dot" style="background: #3572A5;"></span> Python
              </span>
            </div>
          </div>

          <!-- automathon -->
          <div class="repo-card">
            <div class="row" style="align-items: center; gap: 5px; margin-bottom: 3px;">
              <span class="repo-name">automathon</span>
            </div>
            <div class="repo-desc" style="margin-bottom: 5px;">Python library for simulating &amp; visualizing finite automata</div>
            <div class="row muted" style="align-items: center; font-size: 0.68rem;">
              <span class="row" style="align-items: center; gap: 4px;">
                <span class="lang-dot" style="background: #3572A5;"></span> Python
              </span>
            </div>
          </div>

          <!-- robinzhon -->
          <div class="repo-card">
            <div class="row" style="align-items: center; gap: 5px; margin-bottom: 3px;">
              <span class="repo-name">robinzhon</span>
            </div>
            <div class="repo-desc" style="margin-bottom: 5px;">Minimal, high-performance Python helpers for concurrent S3 transfers</div>
            <div class="row muted" style="align-items: center; font-size: 0.68rem;">
              <span class="row" style="align-items: center; gap: 4px;">
                <span class="lang-dot" style="background: #3572A5;"></span> Python
              </span>
            </div>
          </div>

          <!-- fastiter -->
          <div class="repo-card">
            <div class="row" style="align-items: center; gap: 5px; margin-bottom: 3px;">
              <span class="repo-name">fastiter</span>
            </div>
            <div class="repo-desc" style="margin-bottom: 5px;">Parallel iterators for Python 3.14+</div>
            <div class="row muted" style="align-items: center; font-size: 0.68rem;">
              <span class="row" style="align-items: center; gap: 4px;">
                <span class="lang-dot" style="background: #3572A5;"></span> Python
              </span>
            </div>
          </div>

        </div>

        <!-- Contributions -->
        <div style="border-top: 1px solid #21262d; padding-top: 10px;">
          <div class="row" style="align-items: center; gap: 6px; margin-bottom: 6px;">
            <span style="width: 8px; height: 8px; border-radius: 50%; background: #e6edf3; display: inline-block;"></span>
            <span style="color: #e6edf3; font-weight: 600; font-size: 0.82rem;">Contribuciones a otros proyectos</span>
          </div>
          <div style="display: flex; flex-wrap: wrap; gap: 6px;">
            <span style="background: #161b22; border: 1px solid #30363d; border-radius: 6px; padding: 4px 10px; font-size: 0.7rem; color: #c9d1d9;">
              <span style="color: #58a6ff;">rust-lang/rust</span> &nbsp;·&nbsp; 2 PRs merged
            </span>
            <span style="background: #161b22; border: 1px solid #30363d; border-radius: 6px; padding: 4px 10px; font-size: 0.7rem; color: #c9d1d9;">
              <span style="color: #58a6ff;">fluidattacks/makes</span> &nbsp;·&nbsp; ex‑maintainer
            </span>
            <span style="background: #161b22; border: 1px solid #30363d; border-radius: 6px; padding: 4px 10px; font-size: 0.7rem; color: #c9d1d9;">
              <span style="color: #58a6ff;">cattle-ops/terraform-aws-gitlab-runner</span> &nbsp;·&nbsp; PR merged
            </span>
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<style scoped src="../styles/github-card.css"></style>
