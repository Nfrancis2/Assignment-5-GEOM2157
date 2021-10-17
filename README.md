# Assignment-5-GEOM2157
My assignment 

my script will reclassify a DEM raster into 1 or 0 based on flood levels. This will help to to identify areas that are flooded = 0 or not flooded = 1

    from qgis.core import QgsProcessing
    from qgis.core import QgsProcessingAlgorithm
    from qgis.core import QgsProcessingMultiStepFeedback
    from qgis.core import QgsProcessingParameterRasterLayer
    from qgis.core import QgsProcessingParameterVectorLayer
    from qgis.core import QgsProcessingParameterFeatureSink
    from qgis.core import QgsExpression
    import processing


    class ReclassifyRasterAccordingToFloodLevel(QgsProcessingAlgorithm):

    def initAlgorithm(self, config=None):
      self.addParameter(QgsProcessingParameterRasterLayer('RastertoReclassify', 'Raster to Reclassify', defaultValue=None))
      #self.addParameter(QgsProcessingParameterNumber( 'INPUT_floodlevel', 'INPUT floodlevel', type=QgsProcessingParameterNumber.integer))
    def processAlgorithm(self, parameters, context, model_feedback):


    # Reclassify with table
    # INPUT_floodlevel is an integer value of the flood level in units relative to the DEM layer 
    # e.g. if the DEM layer is in meters the INPUT_floodlevel
    alg_params = {
        'DATA_TYPE': 5,
        'INPUT_RASTER': parameters['RastertoReclassify'],
        'RASTER_BAND' : 1,
        'TABLE' : [-10,INPUT_floodlevel,0,INPUT_floodlevel+1,788,1], 
        'NO_DATA' : -9999, 
        'RANGE_BOUNDARIES' : 0,
        'NODATA_FOR_MISSING' : False, 
        'OUTPUT':QgsProcessing.TEMPORARY_OUTPUT}


    outputs['ReclassifyWithTable'] = processing.run('native:reclassifybytable', alg_params, context=context, feedback=feedback, is_child_algorithm=True)

    feedback.setCurrentStep(1)
    if feedback.isCanceled():
        return {}
    return results


    def name(self):
      return 'Reclassify Raster according to flood level'

    def displayName(self):
      return 'Reclassify Raster according to flood level'

    def shortHelpString(self):
      return ' '

    def createInstance(self):
      return ReclassifyRasterAccordingToFloodLevel()
