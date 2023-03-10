# Example of the rubocop issue where disabling line length checks causes Style/IfUnlessModifier to fail

This is a miniman reproduction of the issue I've been seeing on our main repo.

## Steps to reproduce

1. Clone the repo
2. Run `bundle`, only rubocop will be installed
3. Run rubocop: `bundle exec rubocop -c rubocop.yml`
4. You will see the output:
      ```
      Inspecting 2 files
      .C

      Offenses:

      len.rb:6:121: C: Layout/LineLength: Line is too long. [869/120]
        "Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. Nemo enim ipsam voluptatem quia voluptas sit aspernatur aut odit aut fugit, sed quia consequuntur magni dolores eos qui ratione voluptatem sequi nesciunt. Neque porro quisquam est, qui dolorem ipsum quia dolor sit amet, consectetur, adipisci velit, sed quia non numquam eius modi tempora incidunt ut labore et dolore magnam aliquam quaerat voluptatem. Ut enim ad minima veniam, quis nostrum exercitationem ullam corporis suscipit laboriosam, nisi ut aliquid ex ea commodi consequatur? Quis autem vel eum iure reprehenderit qui in ea voluptate velit esse quam nihil molestiae consequatur, vel illum qui dolorem eum fugiat quo voluptas nulla pariatur?"
                                                                                                                              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

      2 files inspected, 1 offense detected
      ```
5. Uncomment the `Layout/LineLength` config in `rubocop.yml` to disable line length checks.
6. Re-run rubocop: `bundle exec rubocop -c rubocop.yml`
7. You will see that the `Layout/LineLength` failure has gone as expected, but a new `Style/IfUnlessModifier` failure is reported even though no config was changed for that cop.
      ```
      Inspecting 2 files
      .C

      Offenses:

      len.rb:5:1: C: [Correctable] Style/IfUnlessModifier: Favor modifier unless usage when having a single-line body. Another good alternative is the usage of control flow &&/||.
      unless somevar == false
      ^^^^^^

      2 files inspected, 1 offense detected, 1 offense autocorrectable
      ```
