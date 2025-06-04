# Data-Driven Magic API Simulation

This project implements a simulated API for Harry Potter character information using a data-driven approach.

## Overview

The simulation provides a RESTful API that serves information about Harry Potter characters. The data is sourced from a CSV file (`hpcharacters.csv`) and is served through configured endpoints.

## Configuration Structure

The main configuration file `DataMagic/DataDrivenMagic.yaml` defines:

- Schema Version: SimV1
- Resources Configuration
- Connection Settings:
  - Port: 26414
  - Endpoint: http://localhost:26414

## Resources

The simulation uses resources to share data at runtime. In our configuration:

```yaml
resources:
- file: hpCharacters.csv
  name: hpcharacters
```

### Resource Properties

Resources in the simulation follow these key properties:

- **name**: A mandatory unique identifier for the resource (in our case, "hpcharacters")
- **file**: The data source file path
- **ref**: Used in service steps to reference this resource
- **where/one**: Properties used to restrict and query values from the resource
  - `where` returns all matching values
  - `one` returns the first matching value

### Resource Usage in Services

The MagicUsersService uses the resource for data lookups:

```yaml
- direction: Out
  insert:
  - jsonPath: Name
    value: "{from[hpcharacters][Name][id='{B[id]}']}"
```

This pattern shows how the service:
1. References the "hpcharacters" resource
2. Looks up values using the ID parameter
3. Returns specific fields using JsonPath syntax

## Services

### MagicUsersService

This service handles character information requests.

#### Endpoints:

- `GET /magic/{id}`: Retrieves character information by ID

#### Response Fields:
- Name
- Gender
- Job
- House
- Wand
- Patronus
- Species
- Loyalty
- Skills
- Birth
- Death

### CallMagicUsersService

This service implements the client-side testing of the Magic Users API.

- Tests the `/magic/1` endpoint
- Verifies 200 OK response

## Data Source

The service uses `hpcharacters.csv` as its primary data source. This file contains detailed information about Harry Potter characters with the following fields:

### CSV Structure
The data file contains the following columns:

1. Id: Unique identifier for each character
2. Name: Full character name
3. Gender: Character's gender
4. Job: Character's occupation or role
5. House: Hogwarts house affiliation
6. Wand: Wand description including length, wood type, and core
7. Patronus: Character's Patronus form
8. Species: Character's species (Human, Ghost, Werewolf, etc.)
9. Blood status: Pure-blood, Half-blood, Muggle-born, etc.
10. Hair colour: Character's hair color
11. Eye colour: Character's eye color
12. Loyalty: Character's allegiances and affiliations
13. Skills: Notable abilities and talents
14. Birth: Character's birth date
15. Death: Character's death date (if applicable)

### Data Examples

#### Sample Character Entry (Harry Potter):
```json
{
  "Id": 1,
  "Name": "Harry James Potter",
  "Gender": "Male",
  "Job": "Student",
  "House": "Gryffindor",
  "Wand": "11\" Holly phoenix feather",
  "Patronus": "Stag",
  "Species": "Human",
  "Loyalty": "Albus Dumbledore | Dumbledore's Army | Order of the Phoenix | Hogwarts School of Witchcraft and Wizardry",
  "Skills": "Parseltongue| Defence Against the Dark Arts | Seeker",
  "Birth": "31 July 1980",
  "Death": ""
}
```

## Getting Started

1. Ensure the `hpcharacters.csv` file is present in the correct location
2. The service will automatically start on port 26414
3. Access character information through the `/magic/{id}` endpoint

## Example Request

```http
GET http://localhost:26414/magic/1
```

The response will include all available information about the requested character in JSON format.

## Data Statistics

The dataset includes:
- Total Characters: 138
- Houses Represented: Gryffindor, Slytherin, Ravenclaw, Hufflepuff
- Species Types: Human, Ghost, Werewolf, Half-Giant, Centaur, and more
- Time Period: Characters from the Hogwarts founders (pre-976) to the next generation (post-2005) 