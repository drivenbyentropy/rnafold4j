# RNAFold4J
## A Java port of RNAFold from the ViennaRNA package

RNAFold4J is a port of the RNAFold program from the ViennaRNA package (v1.8.5) for Java. It requires no external depencies is hence platform independent. In addition, an API is available which allows for the integration of RNAFold4J into bioinformatics pipelines in a programmatic manner. 

**Disclaimer: RNAFold is implemented and developed by Ivo L Hofacker et al. as part of the ViennaRNA package. All intellectual credits of this work go to the original authors and the Institute for Theoretical Chemistry of the University of Vienna. My only contribution is the adaptation of the C source code to Java.**

## Usage
```
java -jar rnafold4j [-p[0|2]] [-C] [-T temp] [-4] [-d[2|3]] [-noGU] [-noCloseGU]
                    [-noLP] [-e e_set] [-nsp pairs] [-S scale]
                    [-noconv] [-circ] [-o outputFolder]
```
where the individual options correspond to the RNAFold parameters (see the [official manual](https://www.tbi.univie.ac.at/RNA/RNAfold.1.html) for a detailed description). 

## Differences from the orignial implementation
* MEA prediction is currently not implemented.
* Dot plots are currently not implemented. Instead, base pair probabilities are written to text files when using the `-p` option.

## RNAFold4J API
If you want to integrate `RNAFold4J` in your own pipeline, you can use the provided API as such
```java
  import rnafold4j.MFEData;
  import rnafold4j.RNAFoldAPI;

  public static void main(String[] args) {
		
    // The sequence to be predicted
    byte[] seq = new String("GUGACGUGUGCAAAUGUGACGUGUGCAAAUGUGACGUGUGCAAAU").getBytes();
		
    // Create a new instance of RNAFold4J
    RNAFoldAPI rfa = new RNAFoldAPI();
    
    // Predict the MFE and corresponding structure
    MFEData mfe = rfa.getMFE(seq);
    System.out.println(String.format("Sequence:  %s\nStructure: %s \nMFE:       %f", new String(seq), new String(mfe.structure), mfe.mfe));

    // Predict the base pair probability matrix (equivalent to using the -p option).
    // Returns linearized upper trianguar matrix
    double[] bppm = rfa.getBppm(seq);
  }
```

## Precomiled Jar File
Please see the [release section](https://github.com/drivenbyentropy/rnafold4j/releases) for the most recent version of RNAFold4J.

## Compile From Source
RNAFold4j is a maven project:
```
git clone https://github.com/drivenbyentropy/rnafold4j.git
cd rnafold4j
mvn package
```
The compiled jar file can then be found in `target\rnafold4j.jar`
