<!-- README.md is auto-generated from README.md.template -->

# action-template

## Usage

This README.md is generated from [a template](README.md.template);.

```yaml
name: Example
on:
  workflow_dispatch:

jobs:
  run-templating:
    runs-on: ubuntu-latest
    steps:
      - name: Generate token
        id: generate_token
        uses: tibdex/github-app-token@v1
        with:
          app_id: ${{ secrets.APP_ID }}
          private_key: ${{ secrets.APP_PRIVATE_KEY }}
      - name: Checkout action
        uses: actions/checkout@v2
        with:
          repository: EffortGames/action-template
          token: ${{ steps.generate_token.outputs.token }}
          path: ./action
      - uses: actions/setup-python@v2
        with:
          python-version: 3.x
      - name: Create README.md
        env:
          GITHUB_CONTEXT: ${{ toJson(github) }}
        run: |
          cd ./action
          echo "github.json:"
          echo "$GITHUB_CONTEXT" | tee github.json
          pip install -r requirements.txt
          ./main.py README.md.template github.json .github/workflows/example.yml > README.md

          git config --global user.email "automation@effortgames.nl"
          git config --global user.name "EffortGames Automation"
          git add README.md
          git commit -am"Update README.md"
          git push origin master
      - id: foo
        uses: ./action
        with:
          who-to-greet: 'Mona the Octocat'
```
