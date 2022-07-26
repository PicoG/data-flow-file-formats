# Data Flow File Formats
Experimental work in possible file formats for storing data-flow programs.

Here is an example of specifying the functional (non-cosmetic) parts of a LabVIEW VI (Virtual Instrument).

![count_up.vi](https://user-images.githubusercontent.com/381432/180230602-9dd249de-6651-4cac-aef7-9076fd1b9ed6.png)

> VSCode can validate and autocomplete YAML using a [json schema](https://json-schema.org/) file such as the [yml/vi_lint.json](https://github.com/PicoG/data-flow-file-formats/blob/main/yml/vi_lint.json) in this repository. ([see detailed instructions](https://dev.to/brpaz/how-to-create-your-own-auto-completion-for-json-and-yaml-files-on-vs-code-with-the-help-of-json-schema-k1i)).

[count_up.vi.yml](yml/count_up.vi.yml)
```yaml
# count_up.vi.yml
#
# A simple VI that counts up until the user presses a "Stop" button.
#
# It has the following parts:
#   - a While Loop that runs until the user presses a Stop Button
#   - an LED indicator to show if it's running (wired to negated Stop Button value)
#   - a loop counter indicator wired up to the while loop's iteration terminal
#   - a small 100ms loop delay constant wired into a "Wait (ms)" timer

type: virtual_instrument
name: my_vi

# we can set some VI properties related to execution
execution:
  reentrant: false
  inline: false

# controls on the front panel
controls:
  - name: stop_button
    type: button

  - name: is_running
    type: led
    is_indicator: true

  - name: num_iterations
    type: numeric
    is_indicator: true

nodes:
  - name: main_loop
    type: while_loop
    iteration_terminal:
      visible: true

    diagram:
      nodes:
        # This value drives the loop rate
        - name: loop_rate
          type: numeric_constant

        # Wait MS Timer
        - type: wait_ms # use type as name/id if it'll be unique for the VI

        # Stop Button's terminal on Block Diagram
        - name: stop_button # needs to be the same as the control name
          type: control_terminal # how we know it's a control_terminal type

        # While Loop's return selector
        - name: stop_if_true
          type: loop_return_selector
          mode: stop_if_true

        # Not node to invert the Stop Button's Boolean value for LED
        - name: "not"
          type: not

        # LED is_running indicator
        - name: is_running # needs to be the same as the control name
          type: control_terminal # how we know it's a control_terminal type

        # While Loop's iteration terminal
        - type: loop_iteration

        # # A local variable of the "is_running" control
        # - type:     # should read/write locals be different types?
        #   control: is_running

      wires:
        - source:
            node: loop_rate
          sinks:
            - node: wait_ms
              terminal: milliseconds_to_wait

        - source:
            node: stop_button
          sinks:
            - node: stop_if_true # term not specified b/c node only has 1 term
            - node: not
              terminal: x

        - source:
            node: not
            terminal: not_x
          sinks:
            - node: is_running # this indicator terminal only has 1 term

        - source:
            node: loop_iteration
          sinks:
            - node: num_iterations
            
```
