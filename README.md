# 🦠 COVID-19 Epidemic Simulation in Kenya using the SIR Model

## 📌 What?
This project simulates the spread of COVID-19 in Kenya using the SIR (Susceptible–Infected–Recovered) compartmental model. It analyzes how vaccination coverage and transmission rates impact the epidemic curve, with the aim of understanding public health intervention strategies.

---

## 🛠️ How?
We implemented the simulation using R with the following steps:
- Defined the SIR differential equations using the `deSolve` package.
- Modeled a population of 50 million with an initial infection count of 500.
- Simulated 180 days of the epidemic under three scenarios:
  1. No vaccination
  2. 20% population vaccinated
  3. 40% population vaccinated
- Visualized disease progression using `ggplot2`.

 Tools & Packages: `deSolve`, `tidyverse`, `DiagrammeR`, `ggplot2`

---

## 🎯 Why?
Understanding how COVID-19 spreads and how early interventions affect the outbreak is essential for:
- Planning hospital resource allocation
- Designing vaccination policies
- Informing government response

This project demonstrates the power of simulation in epidemiological decision-making, highlighting how increased vaccination and reduced transmission rates flatten the epidemic curve.

---

## 💡 Key Code Snippets

### ✅ SIR Model Function & Solver

```r
sir_model <- function(time, state, parameters) {
  with(as.list(c(state, parameters)), {
    N <- S + I + R
    lambda <- beta * I / N
    dS <- -lambda * S
    dI <- lambda * S - gamma * I
    dR <- gamma * I
    return(list(c(dS, dI, dR)))
  })
}

output <- as.data.frame(ode(y = initial_state_values, times = times, func = sir_model, parms = parameters))

### 💉 Impact of 40% Vaccination

The following R code simulates the effect of increasing vaccination coverage to 40% of the population. Vaccinated individuals are moved to the "Recovered" compartment at the beginning of the simulation.

```r
vaccination_coverage_high <- 0.4
initial_susceptible_high_vacc <- (1 - vaccination_coverage_high) * population_size
initial_recovered_high_vacc <- vaccination_coverage_high * population_size
initial_state_values_high_vacc <- c(S = initial_susceptible_high_vacc, I = initial_infected, R = initial_recovered_high_vacc)

output_high_vacc <- as.data.frame(ode(
  y = initial_state_values_high_vacc,
  times = times,
  func = sir_model,
  parms = parameters
))

## 🧗 Challenges Faced, How I Solved Them & Lessons Learned

<details>
<summary><strong>Challenge 1: Handling Stiff Differential Equation Syntax in R</strong></summary>

- **Problem**: I initially struggled with setting up the `ode()` function from the `deSolve` package due to unfamiliar syntax and the strict format it requires for model functions.  
- **Solution**: I studied examples from official documentation and simplified the `sir_model` function using `with(as.list(...))`.  
- **Lesson Learned**: Understanding how to break down complex functions into smaller logical steps makes debugging much easier and improves comprehension of dynamic systems.

</details>

<details>
<summary><strong>Challenge 2: Plot Rendering Issues in RMarkdown</strong></summary>

- **Problem**: Some `ggplot2` plots wouldn't render in RMarkdown.  
- **Solution**: Adjusted code chunk settings and reshaped the data using `pivot_longer()`.  
- **Lesson Learned**: Clean chunk setup and tidy data are key to effective reporting in RMarkdown.

</details>

<details>
<summary><strong>Challenge 3: Comparing Multiple Vaccination Scenarios</strong></summary>

- **Problem**: Difficult to interpret results across separate plots.  
- **Solution**: Combined plots using `facet_wrap()` and labeled scenarios.  
- **Lesson Learned**: Visual storytelling improves interpretability and communication.

</details>

<details>
<summary><strong>Challenge 4: Interpreting Parameters (Beta & Gamma)</strong></summary>

- **Problem**: Translating β and γ into public health terms was challenging.  
- **Solution**: Simulated varying transmission rates and interpreted resulting curve shapes.  
- **Lesson Learned**: Experimenting with parameter values bridges the gap between math and real-world application.

</details>

