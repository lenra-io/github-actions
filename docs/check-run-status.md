# check-run-status.yml

## How to use

```yml
  pre-coverage-feedback:
    name: Prepare for Coverage Feedback
    uses: lenra-io/github-actions/.github/workflows/check-run-status.yml@main
    with:
      name: 'Coverage Report'

  test:
    name: Test Flutter App
    # Call the reusable workflow that runs test and generate a test coverage report into artifacts, artifact's name will be in outputs.
    uses: lenra-io/github-actions/.github/workflows/test-flutter.yml@main

  coverage:
    name: Read Coverage Report
    needs: test
    uses: lenra-io/github-actions/.github/workflows/coverage-report.yml@main
    with:
      artifact-name: ${{ needs.test.outputs.artifact-name }}
      artifact-path: lcov.info
      min-lines-coverage: 20
  
  coverage_feedback:
    name: Feedback on Coverage
    needs: [pre-coverage-feedback, coverage]
    uses: lenra-io/github-actions/.github/workflows/check-run-status.yml@main
    if: ${{ needs.pre-coverage-feedback.result && always() }}
    with:
      check-run-id: ${{ needs.pre-coverage-feedback.outputs.check-run-id }}
      name: 'Coverage Report'
      status: completed
      # Job success or failure
      conclusion: ${{ needs.coverage.result }}
      output_title: 'Coverage Report'
      output_summary: 'Lines coverage at ${{ needs.coverage.outputs.lines-percentage }}% (${{ needs.coverage.outputs.lines-covered }}/${{ needs.coverage.outputs.lines-total }})'
      output_text: |
        lines......: ${{ needs.coverage.outputs.lines-percentage }}% (${{ needs.coverage.outputs.lines-covered }} of ${{ needs.coverage.outputs.lines-total }} lines)
        functions..: ${{ needs.coverage.outputs.functions-percentage }}% (${{ needs.coverage.outputs.functions-covered }} of ${{ needs.coverage.outputs.functions-total }} functions)
        branches...:  ${{ needs.coverage.outputs.branches-percentage }}% (${{ needs.coverage.outputs.branches-covered }} of ${{ needs.coverage.outputs.branches-total }} branches)
```

This example will create a new Run check status in the pull request, and after test got run, it will update it so it will tell the percentage of test coverage.
This can use in many other way to inform the developer of steps of the run to be more clean.
