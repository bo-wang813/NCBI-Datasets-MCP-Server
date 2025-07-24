# NCBI Datasets MCP Server

A Model Context Protocol (MCP) server that provides comprehensive access to the NCBI Datasets API. This server enables seamless integration with NCBI's vast collection of genomic, taxonomic, and biological data through 31 specialized tools.

## Features

- **31 comprehensive tools** covering all major NCBI Datasets functionality
- **9 organized categories** of biological data operations
- **Resource templates** for direct URI-based data access
- **Full TypeScript implementation** with proper error handling
- **Rate limiting and caching** for optimal performance
- **Environment variable configuration** for API keys

## Installation

```bash
npm install
npm run build
```

## Configuration

### Environment Variables

- `NCBI_API_KEY` (optional): Your NCBI API key for higher rate limits and priority access

### MCP Configuration

Add to your MCP settings:

```json
{
  "mcpServers": {
    "ncbi-datasets-server": {
      "command": "node",
      "args": ["/path/to/ncbi-datasets-server/build/index.js"],
      "env": {
        "NCBI_API_KEY": "your_api_key_here"
      }
    }
  }
}
```

## Available Tools

### 🧬 Genome Operations

- `search_genomes` - Search genome assemblies by organism, keywords, or criteria
- `get_genome_info` - Get detailed information for a specific genome assembly
- `get_genome_summary` - Get summary statistics for a genome assembly

### 🧬 Gene Operations

- `search_genes` - Search genes by symbol, name, organism, or location
- `get_gene_info` - Get detailed information for a specific gene
- `get_gene_sequences` - Retrieve sequences for a specific gene

### 🏷️ Taxonomy Operations

- `search_taxonomy` - Search taxonomic information by organism name
- `get_taxonomy_info` - Get detailed taxonomic information for a taxon
- `get_organism_info` - Get organism-specific information and datasets

### 🏗️ Assembly Operations

- `search_assemblies` - Search genome assemblies with detailed filtering
- `get_assembly_info` - Get detailed metadata and statistics for assemblies
- `get_assembly_reports` - Get assembly quality reports and validation info
- `download_genome_data` - Get download URLs for genome data files
- `batch_assembly_info` - Get information for multiple assemblies

### 🦠 Virus Operations

- `search_virus_genomes` - Search viral genome assemblies
- `get_virus_info` - Get detailed information for viral genomes

### 🧪 Protein Operations

- `search_proteins` - Search protein sequences by name or function
- `get_protein_info` - Get detailed information for specific proteins

### 📝 Annotation Operations

- `get_genome_annotation` - Get annotation information for assemblies
- `search_genome_features` - Search for specific genomic features

### 🔬 Comparative Genomics

- `compare_genomes` - Compare two or more genome assemblies
- `find_orthologs` - Find orthologous genes across organisms

### 🧬 Sequence Operations

- `get_sequence_data` - Retrieve sequence data for genomes/genes/proteins
- `blast_search` - Perform BLAST search against NCBI databases

### 🌳 Phylogenetic Operations

- `get_phylogenetic_tree` - Get phylogenetic tree data for organisms
- `get_taxonomic_lineage` - Get complete taxonomic lineage

### 📊 Statistics Operations

- `get_database_stats` - Get statistics about NCBI Datasets content
- `search_by_bioproject` - Search datasets by BioProject accession
- `search_by_biosample` - Search datasets by BioSample accession

### ✅ Quality Control

- `get_assembly_quality` - Get quality metrics for genome assemblies
- `validate_sequences` - Validate sequence data and check for issues

## Usage Examples

### Using with Cline

Once configured, you can use the tools directly in Cline by referencing the server name and tool:

**Search for E. coli Genomes:**

```
Use the ncbi-datasets-server to search for E. coli genomes with complete assembly level, limiting to 10 results using tax_id 511145
```

**Get Detailed Genome Information:**

```
Get detailed information for genome assembly GCF_000005845.2 including annotation data using ncbi-datasets-server
```

**Search for BRCA1 Gene:**

```
Search for the BRCA1 gene in Homo sapiens using ncbi-datasets-server, limit to 5 results
```

### MCP Protocol Usage

For direct MCP client integration, use JSON-RPC format:

**Search Genomes:**

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "tools/call",
  "params": {
    "name": "search_genomes",
    "arguments": {
      "tax_id": 511145,
      "assembly_level": "complete",
      "max_results": 10
    }
  }
}
```

**Get Genome Information:**

```json
{
  "jsonrpc": "2.0",
  "id": 2,
  "method": "tools/call",
  "params": {
    "name": "get_genome_info",
    "arguments": {
      "accession": "GCF_000005845.2",
      "include_annotation": true
    }
  }
}
```

**Search Genes:**

```json
{
  "jsonrpc": "2.0",
  "id": 3,
  "method": "tools/call",
  "params": {
    "name": "search_genes",
    "arguments": {
      "gene_symbol": "BRCA1",
      "organism": "Homo sapiens",
      "max_results": 5
    }
  }
}
```

### Resource Access

You can also access data directly using resource URIs:

```json
{
  "jsonrpc": "2.0",
  "id": 4,
  "method": "resources/read",
  "params": {
    "uri": "ncbi://genome/GCF_000005845.2"
  }
}
```

## Resource Templates

The server provides resource templates for direct data access:

- `ncbi://genome/{accession}` - Complete genome assembly information
- `ncbi://gene/{gene_id}` - Gene information with annotations
- `ncbi://taxonomy/{tax_id}` - Taxonomic classification and lineage
- `ncbi://assembly/{assembly_accession}` - Assembly metadata and statistics
- `ncbi://search/{data_type}/{query}` - Search results for specified queries

## API Rate Limits

- **Without API key**: 3 requests per second
- **With API key**: 10 requests per second with priority access

To obtain an API key, visit: https://www.ncbi.nlm.nih.gov/account/settings/

## Error Handling

The server implements comprehensive error handling:

- **Network errors**: Automatic retry with exponential backoff
- **Rate limiting**: Intelligent request queuing and throttling
- **Invalid parameters**: Clear validation error messages
- **API errors**: Detailed error reporting with context

## Data Sources

This server accesses data from:

- **NCBI Datasets API v2**: Primary genomic and assembly data
- **NCBI Taxonomy**: Taxonomic classifications and lineages
- **NCBI Gene**: Gene annotations and sequences
- **NCBI Assembly**: Assembly metadata and quality metrics
- **NCBI BioProject/BioSample**: Project and sample information

## License

MIT License - see LICENSE file for details.

## Contributing

Contributions are welcome! Please feel free to submit issues, feature requests, or pull requests.

## Support

For issues related to:

- **Server functionality**: Open an issue in this repository
- **NCBI data**: Consult NCBI Datasets documentation
- **API access**: Contact NCBI support for API-related questions
